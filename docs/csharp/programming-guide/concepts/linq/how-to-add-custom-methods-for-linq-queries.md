---
title: 如何为 LINQ 查询添加自定义方法 (C#)
description: 了解如何通过向 C# 中的 IEnumerable<T> 接口中添加扩展方法来扩展 LINQ 查询的语法。
ms.date: 08/26/2020
ms.assetid: 1a500f60-2e10-49fb-8b2a-d8d08e4817cb
ms.openlocfilehash: 768882fce40a2fc6e018f24c8928341e7c65bc4b
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89122420"
---
# <a name="how-to-add-custom-methods-for-linq-queries-c"></a>如何为 LINQ 查询添加自定义方法 (C#)

通过向 <xref:System.Collections.Generic.IEnumerable%601> 接口添加扩展方法扩展可用于 LINQ 查询的方法集。 例如，除了标准平均值或最大值运算，还可创建自定义聚合方法，从一系列值计算单个值。 此外，还可创建一种方法，用作值序列的自定义筛选器或特定数据转换，并返回新的序列。 <xref:System.Linq.Enumerable.Distinct%2A>、<xref:System.Linq.Enumerable.Skip%2A> 和 <xref:System.Linq.Enumerable.Reverse%2A> 就是此类方法的示例。

扩展 <xref:System.Collections.Generic.IEnumerable%601> 接口时，可以将自定义方法应用于任何可枚举集合。 有关详细信息，请参阅[扩展方法](../../classes-and-structs/extension-methods.md)。

## <a name="adding-an-aggregate-method"></a>添加聚合方法

聚合方法可从一组值计算单个值。 LINQ 提供多个聚合方法，包括 <xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.Min%2A> 和 <xref:System.Linq.Enumerable.Max%2A>。 可以通过向 <xref:System.Collections.Generic.IEnumerable%601> 接口添加扩展方法来创建自己的聚合方法。

下面的代码示例演示如何创建名为 `Median` 的扩展方法来计算类型为 `double` 的数字序列的中间值。

:::code language="csharp" source="./snippets/LinqExtensions.cs" ID="LinqExtensionClass":::

使用从 <xref:System.Collections.Generic.IEnumerable%601> 接口调用其他聚合方法的方式为任何可枚举集合调用此扩展方法。

下面的代码示例说明如何为类型 `double` 的数组使用 `Median` 方法。

:::code language="csharp" source="./snippets/Program.cs" ID="MedianUsage":::

### <a name="overloading-an-aggregate-method-to-accept-various-types"></a>重载聚合方法以接受各种类型

可以重载聚合方法，以便其接受各种类型的序列。 标准做法是为每种类型都创建一个重载。 另一种方法是创建一个采用泛型类型的重载，并使用委托将其转换为特定类型。 还可以将两种方法结合。

#### <a name="to-create-an-overload-for-each-type"></a>为每种类型创建重载

可以为要支持的每种类型创建特定重载。 下面的代码示例演示 `int` 类型的 `Median` 方法的重载。

:::code language="csharp" source="./snippets/LinqExtensions.cs" ID="IntOverload":::

现在便可以为 `integer` 和 `double` 类型调用 `Median` 重载了，如以下代码中所示：

:::code language="csharp" source="./snippets/Program.cs" ID="OverloadUsage":::

#### <a name="to-create-a-generic-overload"></a>创建一般重载

还可以创建接受泛型对象序列的重载。 此重载采用委托作为参数，并使用该参数将泛型类型的对象序列转换为特定类型。

下面的代码展示 `Median` 方法的重载，该重载将 <xref:System.Func%602> 委托作为参数。 此委托采用泛型类型 T 的对象，并返回类型 `double` 的对象。

:::code language="csharp" source="./snippets/LinqExtensions.cs" ID="GenericOverload":::

现在，可以为任何类型的对象序列调用 `Median` 方法。 如果类型没有它自己的方法重载，必须手动传递委托参数。 在 C# 中，可以使用 lambda 表达式实现此目的。 此外，仅限在 Visual Basic 中，如果使用 `Aggregate` 或 `Group By` 子句而不是方法调用，可以传递此子句范围内的任何值或表达式。

下面的代码示例演示如何为整数数组和字符串数组调用 `Median` 方法。 对于字符串，将计算数组中字符串长度的中值。 该示例演示如何将 <xref:System.Func%602> 委托参数传递给每个用例的 `Median` 方法。

:::code language="csharp" source="./snippets/Program.cs" ID="GenericUsage":::

## <a name="adding-a-method-that-returns-a-sequence"></a>添加返回序列的方法

可以使用会返回值序列的自定义查询方法来扩展 <xref:System.Collections.Generic.IEnumerable%601> 接口。 在这种情况下，该方法必须返回类型 <xref:System.Collections.Generic.IEnumerable%601> 的集合。 此类方法可用于将筛选器或数据转换应用于值序列。

下面的示例演示如何创建名为 `AlternateElements` 的扩展方法，该方法从集合中第一个元素开始按相隔一个元素的方式返回集合中的元素。

:::code language="csharp" source="./snippets/LinqExtensions.cs" ID="SequenceElement":::

可使用从 <xref:System.Collections.Generic.IEnumerable%601> 接口调用其他方法的方式对任何可枚举集合调用此扩展方法，如下面的代码中所示：

:::code language="csharp" source="./snippets/Program.cs" ID="SequenceUsage":::

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic.IEnumerable%601>
- [扩展方法](../../classes-and-structs/extension-methods.md)
