---
title: 在 .NET for Apache Spark 中使用广播变量
description: 了解如何在 .NET for Apache Spark 应用程序中使用广播变量。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: ca6dab01cbd639594da0b51f145272a9a150e93c
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687748"
---
# <a name="use-broadcast-variables-in-net-for-apache-spark"></a>在 .NET for Apache Spark 中使用广播变量

本文介绍如何在 .NET for Apache Spark 中使用广播变量。 [Apache Spark 中的广播变量](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables)是一种机制，可实现在应为只读的执行程序之间共享变量。 借助广播变量即可在每台计算机上缓存只读变量，无需将其副本与任务一起交付。 可以使用广播变量高效地为每个节点提供大型输入数据集的副本。

由于数据仅发送一次，因此相较于随每个任务一起交付给执行程序的局部变量，广播变量具有性能优势。 请参阅[官方广播变量文档](https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#broadcast-variables)，更深入地了解广播变量及其使用原因。

## <a name="create-broadcast-variables"></a>创建广播变量

若要创建广播变量，请为任何变量 `v` 调用 `SparkContext.Broadcast(v)`。 广播变量是以变量 `v` 为中心的包装器，可以通过调用 `Value()` 方法访问它的值。

在以下代码片段中，创建了字符串变量 `v`，并在调用 `SparkContext.Broadcast(v)` 时创建了广播变量 `bv`。 请注意，`Broadcast` 的类型参数（即字符串）与广播的变量的类型匹配。 用户定义的函数 (UDF) 返回 `bv` 的值。

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

Func<Column, Column> udf = Udf<string, string>(
    str => $"{str}: {bv.Value()}");
```

## <a name="delete-broadcast-variables"></a>删除广播变量

可以通过调用 `Destroy()` 方法从所有执行程序中删除广播变量。

```csharp
bv.Destroy();
```

`Destroy()` 可删除与广播变量相关的所有数据和元数据，应谨慎使用。 广播变量一经销毁，即无法再次使用。

## <a name="limit-broadcast-variable-scope-in-udfs"></a>在 UDF 中限制广播变量范围

在 UDF 中使用广播变量时，需要将变量的范围限制为仅引用该变量的 UDF。 [UDF 使用指南](udf-guide.md)详细描述了这一现象。 对广播变量调用 `Destroy()` 时，范围特别重要。

如果已销毁的广播变量对其他 UDF 可见或可通过其他 UDF 访问，则即使所有 UDF 未对其进行引用，也会将其选中进行序列化。 .NET for Apache Spark 无法序列化已销毁的广播变量，进而导致错误。 下面的代码片段演示了这一错误：

```csharp
string v = "Variable to be broadcasted";
Broadcast<string> bv = SparkContext.Broadcast(v);

// Using the broadcast variable in a UDF:
Func<Column, Column> udf1 = Udf<string, string>(
    str => $"{str}: {bv.Value()}");

// Destroying bv
bv.Destroy();

// Calling udf1 after destroying bv throws the following expected exception:
// org.apache.spark.SparkException: Attempted to use Broadcast(0) after it was destroyed
df.Select(udf1(df["_1"])).Show();

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 throws the following (unexpected) exception:
// [Error] [JvmBridge] org.apache.spark.SparkException: Task not serializable
df.Select(udf2(df["_1"])).Show();
```

下面的代码片段演示了如何确保销毁 `bv` 不会由于意外的序列化行为而影响 `udf2`：

```csharp
string v = "Variable to be broadcasted";
// Restricting the visibility of bv to only the UDF referencing it
{
    Broadcast<string> bv = SparkContext.Broadcast(v);

    // Using the broadcast variable in a UDF:
    Func<Column, Column> udf1 = Udf<string, string>(
        str => $"{str}: {bv.Value()}");

    // Destroying bv
    bv.Destroy();
}

// Different UDF udf2 that is not referencing bv
Func<Column, Column> udf2 = Udf<string, string>(
    str => $"{str}: not referencing broadcast variable");

// Calling udf2 works fine as expected
df.Select(udf2(df["_1"])).Show();
```

## <a name="faqs"></a>常见问题解答

**为什么广播变量无法与 .NET Interactive 一起使用？**  
广播变量不能与交互式方案一起使用的原因在于，.NET 交互窗口的设计会将单元格中定义的每个对象都追加到其单元格提交类中，这是因为未标记为可序列化，因此失败，并出现如前所示的同一异常。 有关详细信息，请查看[此文章](dotnet-interactive-udf-issue.md)。

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [在 Windows 上调试 .NET for Apache Spark 应用程序](debug.md)
* [部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件](deploy-worker-udf-binaries.md)
