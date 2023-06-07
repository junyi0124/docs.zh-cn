---
title: 泛型类型（泛型）概述
description: 了解泛型如何充当代码模板，通过该模板可定义类型安全数据结构，无需处理实际数据类型。
author: kuhlenh
ms.author: wiwagn
ms.date: 10/09/2018
ms.openlocfilehash: 5f6e84e23b5bcdcb3dcd742823d83728fb43d195
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557939"
---
# <a name="generic-types-overview"></a>泛型类型概述

在 .NET 中，开发人员随时会使用泛型，有时隐式使用，有时显式使用。 在 .NET 中使用 LINQ 时，你是否曾经注意到，使用的正是 <xref:System.Collections.Generic.IEnumerable%601>？ 或者，你是否曾经看到过有关使用实体框架来与数据库通信的“泛型存储库”在线示例，其中的大多数方法返回 `IQueryable<T>`？ 你可能很想知道，这些示例中的 T  是什么意思，为什么要使用它？

泛型在 .NET Framework 2.0 中首次引入，它本质上是一个“代码模板”，可让开发者定义[类型安全](/previous-versions/dotnet/netframework-4.0/hbzz1a9a(v=vs.100))数据结构，无需处理实际数据类型。 例如，<xref:System.Collections.Generic.List%601> 是一个可以声明的[泛型集合](xref:System.Collections.Generic)，可与 `List<int>`、`List<string>` 或 `List<Person>` 等任何类型结合使用。

为方便理解泛型的作用，让我们看看添加泛型之前和之后的一个特定类：<xref:System.Collections.ArrayList>。 在 .NET Framework 1.0 中，`ArrayList` 元素属于 <xref:System.Object> 类型。 添加到集合的任何元素都会以静默方式转换为 `Object`。 从列表读取元素时，会发生相同的情况。 此过程称为[装箱和取消装箱](../csharp/programming-guide/types/boxing-and-unboxing.md)，它会影响性能。 但除了性能之外，在编译时无法确定列表中的数据的类型，这会形成一些脆弱的代码。 泛型解决了此问题，它可以定义每个列表实例将要包含的数据类型。 例如，只能将整数添加到 `List<int>`，只能将人员添加到 `List<Person>`。

泛型还可以在运行时使用。 运行时知道你要使用的数据结构类型，并可以更高效地将数据结构存储在内存中。

下面的示例是一个小程序，演示了在运行时如何有效地了解数据结构类型：

```csharp
  using System;
  using System.Collections;
  using System.Collections.Generic;
  using System.Diagnostics;

  namespace GenericsExample {
    class Program {
      static void Main(string[] args) {
        //generic list
        List<int> ListGeneric = new List<int> { 5, 9, 1, 4 };
        //non-generic list
        ArrayList ListNonGeneric = new ArrayList { 5, 9, 1, 4 };
        // timer for generic list sort
        Stopwatch s = Stopwatch.StartNew();
        ListGeneric.Sort();
        s.Stop();
        Console.WriteLine($"Generic Sort: {ListGeneric}  \n Time taken: {s.Elapsed.TotalMilliseconds}ms");

        //timer for non-generic list sort
        Stopwatch s2 = Stopwatch.StartNew();
        ListNonGeneric.Sort();
        s2.Stop();
        Console.WriteLine($"Non-Generic Sort: {ListNonGeneric}  \n Time taken: {s2.Elapsed.TotalMilliseconds}ms");
        Console.ReadLine();
      }
    }
  }
```

此程序将生成类似下面的输出：

```console
Generic Sort: System.Collections.Generic.List`1[System.Int32]
 Time taken: 0.0034ms
Non-Generic Sort: System.Collections.ArrayList
 Time taken: 0.2592ms
```

在此处可以看到的第一个优点是，泛型列表的排序比非泛型列表要快得多。 还可以看到，泛型列表的类型是不同的 ([System.Int32])，而非泛型列表的类型已通用化。 由于运行时知道泛型 `List<int>` 的类型是 <xref:System.Int32>，因此可以将列表元素存储在内存中的基础整数数组内；而非泛型 `ArrayList` 必须将每个列表元素强制转换为对象。 如本示例中所示，多余的强制转换会占用时间，降低列表排序的速度。

运行时知道泛型类型的另一个优点是可以改善调试体验。 在 C# 中调试泛型时，可以知道数据结构中每个元素的类型。 如果不使用泛型，则无从知道每个元素的类型是什么。

## <a name="see-also"></a>请参阅

- [C# 编程指南 - 泛型](../csharp/programming-guide/generics/index.md)
