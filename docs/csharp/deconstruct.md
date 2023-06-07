---
title: 析构元组和其他类型
description: 了解如何析构元组和其他类型。
ms.technology: csharp-fundamentals
ms.date: 11/23/2017
ms.assetid: 0b0c4b0f-4a47-4f66-9b8e-f5c63b195960
ms.openlocfilehash: 96168b729ae3ec11d7a38444b8c100bdbff4efbf
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94439698"
---
# <a name="deconstructing-tuples-and-other-types"></a>析构元组和其他类型

元组提供一种从方法调用中检索多个值的轻量级方法。 但是，一旦检索到元组，就必须处理它的各个元素。 按元素逐个执行此操作会比较麻烦，如下例所示。 `QueryCityData` 方法返回一个 3 元组，并通过单独的操作将其每个元素分配给一个变量。

[!code-csharp[WithoutDeconstruction](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple1.cs)]

从对象检索多个字段值和属性值可能同样麻烦：必须按成员逐个将字段值或属性值赋给一个变量。

从 C# 7.0 开始，用户可从元组中检索多个元素，或在单个析构操作中从对象检索多个字段值、属性值和计算值。 析构元组时，将其元素分配给各个变量。 析构对象时，将选定值分配给各个变量。

## <a name="deconstructing-a-tuple"></a>析构元组

C# 提供内置的元组析构支持，可在单个操作中解包一个元组中的所有项。 用于析构元组的常规语法与用于定义元组的语法相似：将要向其分配元素的变量放在赋值语句左侧的括号中。 例如，以下语句将 4 元组的元素分配给 4 个单独的变量：

```csharp
var (name, address, city, zip) = contact.GetAddressInfo();
```

有三种方法可用于析构元组：

- 可以在括号内显式声明每个字段的类型。 以下示例使用此方法来析构由 `QueryCityData` 方法返回的 3 元组。

    [!code-csharp[Deconstruction-Explicit](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple2.cs#1)]

- 可使用 `var` 关键字，以便 C# 推断每个变量的类型。 将 `var` 关键字放在括号外。 以下示例在析构由 `QueryCityData` 方法返回的 3 元组时使用类型推理。

    [!code-csharp[Deconstruction-Infer](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple3.cs#1)]

    还可在括号内将 `var` 关键字单独与任一或全部变量声明结合使用。

    [!code-csharp[Deconstruction-Infer-Some](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple4.cs#1)]

    这很麻烦，不建议这样做。

- 最后，可将元组析构到已声明的变量中。

    [!code-csharp[Deconstruction-Declared](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple5.cs#1)]

请注意，即使元组中的每个字段都具有相同的类型，也不能在括号外指定特定类型。 这会生成编译器错误 CS8136：“析构 var (...) 形式不允许对 var 使用特定类型。”

请注意，还必须将元组的每个元素分配给一个变量。 如果省略任何元素，编译器将生成错误 CS8132，“无法将 ‘x’ 元素的元组析构为 ‘y’ 变量”。

请注意，不能混合析构左侧上现有变量的声明和赋值。 编译器生成错误 CS8184“析构不能混合左侧的声明和表达式”。 当成员包括新声明的和现有的变量。

## <a name="deconstructing-tuple-elements-with-discards"></a>使用弃元析构元组元素

析构元组时，通常只需要关注某些元素的值。 从 C# 7.0 开始，便可利用 C# 对弃元的支持，弃元是一种仅能写入的变量，且其值将被忽略。 在赋值中，通过下划线字符 (\_) 指定弃元。 可弃元任意数量的值，且均由单个弃元  `_` 表示。

以下示例演示了对元组使用弃元时的用法。 `QueryCityDataForYears` 方法返回一个 6 元组，包含城市名称、城市面积、一个年份、该年份的城市人口、另一个年份及该年份的城市人口。 该示例显示了两个年份之间人口的变化。 对于元组提供的数据，我们不关注城市面积，并在一开始就知道城市名称和两个日期。 因此，我们只关注存储在元组中的两个人口数量值，可将其余值作为占位符处理。  

[!code-csharp[Tuple-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

## <a name="deconstructing-user-defined-types"></a>析构用户定义类型

对于非元组类型的解构，C# 不提供内置支持。 但是，用户作为类、结构或接口的创建者，可通过实现一个或多个 `Deconstruct`方法来析构该类型的实例。 该方法返回 void，且要析构的每个值由方法签名中的 [out](language-reference/keywords/out-parameter-modifier.md) 参数指示。 例如，下面的 `Person` 类的 `Deconstruct` 方法返回名字、中间名和姓氏：

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#1)]

然后，可使用类似于以下的分配来析构名为 `p` 的 `Person` 类的实例：

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#2)]

以下示例重载 `Deconstruct` 方法以返回 `Person` 对象的各种属性组合。 单个重载返回：

- 名字和姓氏。
- 名字、中间名和姓氏。
- 名字、姓氏、城市名和省/市/自治区名。

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class2.cs)]

因为可重载 `Deconstruct` 方法来反映通常从对象中提取的数据组，所以应使用独特明确的签名来定义 `Deconstruct` 方法。 如果有多个 `Deconstruct` 方法具有相同数量的 `out` 参数，或具有相同数量和类型的 `out` 参数且顺序不同，则可能会造成混淆。

以下示例中的重载 `Deconstruct` 方法演示一种混淆的可能性。 第一个重载按该顺序返回 `Person` 对象的名字、中间名、姓氏和年龄。 第二个重载仅将姓名信息与年收入一起返回，但名字、中间名和姓氏的顺序不同。 这使得在析构 `Person` 实例时容易混淆参数的顺序。

[!code-csharp[Deconstruct-ambiguity](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-ambiguous.cs)]

## <a name="deconstructing-a-user-defined-type-with-discards"></a>使用弃元析构用户定义类型

就像使用[元组](#deconstructing-tuple-elements-with-discards)一样，可使用弃元来忽略 `Deconstruct` 方法返回的选定项。 每个弃元均由名为“\_”的变量定义，一个析构操作可包含多个弃元。

以下示例将 `Person` 对象析构为四个字符串（名字、姓氏、城市和省/市/自治区），但舍弃姓氏和省/市/自治区。

[!code-csharp[Class-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/class-discard1.cs#1)]

## <a name="deconstructing-a-user-defined-type-with-an-extension-method"></a>使用扩展方法析构用户定义的类型

如果没有创建类、结构或接口，仍可通过实现一个或多个 `Deconstruct` [扩展方法](programming-guide/classes-and-structs/extension-methods.md)来析构该类型的对象，以返回所需值。

以下示例为 <xref:System.Reflection.PropertyInfo?displayProperty=nameWithType> 类定义了两个 `Deconstruct` 扩展方法。 第一个方法返回一组值，指示属性的特征，包括其类型、是静态还是实例、是否为只读，以及是否已编制索引。 第二个方法指示属性的可访问性。 因为 get 和 set 访问器的可访问性可能不同，所以布尔值指示属性是否具有单独的 get 和 set 访问器，如果是，则指示它们是否具有相同的可访问性。 如果只有一个访问器，或者 get 和 set 访问器具有相同的可访问性，则 `access` 变量指示整个属性的可访问性。 否则，get 和 set 访问器的可访问性由 `getAccess` 和 `setAccess` 变量指示。

[!code-csharp[Extension-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-extension1.cs)]

## <a name="see-also"></a>请参阅

- [弃元](discards.md)
- [元组类型](language-reference/builtin-types/value-tuples.md)
