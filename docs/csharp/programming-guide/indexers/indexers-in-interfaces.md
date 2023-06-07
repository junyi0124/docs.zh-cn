---
title: 接口中的索引器 - C# 编程指南
description: 可以在 C# 中的接口上声明索引器。 了解接口索引器的访问器与类索引器的访问器有何差异。
ms.date: 02/08/2020
helpviewer_keywords:
- indexers [C#], in interfaces
- accessors [C#], indexers
ms.assetid: e16b54bd-4a83-4f52-bd75-65819fca79e8
ms.openlocfilehash: ec77843bdf3181a543bd6c02cfb034b21ded1ae7
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303096"
---
# <a name="indexers-in-interfaces-c-programming-guide"></a>接口中的索引器（C# 编程指南）

可以在[接口](../../language-reference/keywords/interface.md)上声明索引器。 接口索引器的访问器与[类](../../language-reference/keywords/class.md)索引器的访问器有所不同，差异如下：

- 接口访问器不使用修饰符。
- 接口访问器通常没有正文。

访问器的用途是指示索引器为读写、只读还是只写。 可以为接口中定义的索引器提供实现，但这种情况非常少。 索引器通常定义 API 来访问数据字段，而数据字段无法在接口中定义。

下面是接口索引器访问器的示例：

[!code-csharp[DefineInterface](~/samples/snippets/csharp/interfaces/indexers.cs#DefineIndexer)]

索引器的签名必须不同于同一接口中声明的所有其他索引器的签名。

## <a name="example"></a>示例

下面的示例演示如何实现接口索引器。

[!code-csharp[Implement](~/samples/snippets/csharp/interfaces/indexers.cs#ImplementInterface)]

[!code-csharp[DefineInterface](~/samples/snippets/csharp/interfaces/indexers.cs#ExampleCode)]

在前面的示例中，可通过使用接口成员的完全限定名来使用显示接口成员实现。 例如

```csharp
string IIndexInterface.this[int index]
{
}
```

但仅当类采用相同的索引签名实现多个接口时，才需用到完全限定名称以避免歧义。 例如，如果 `Employee` 类正在实现接口 `ICitizen` 和接口 `IEmployee`，而这两个接口具有相同的索引签名，则需要用到显式接口成员实现。 即是说以下索引器声明：

```csharp
string IEmployee.this[int index]
{
}
```

在 `IEmployee` 接口中实现索引器，而以下声明：

```csharp
string ICitizen.this[int index]
{
}
```

在 `ICitizen` 接口中实现索引器。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [索引器](./index.md)
- [属性](../classes-and-structs/properties.md)
- [接口](../interfaces/index.md)
