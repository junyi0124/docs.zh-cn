---
title: 可以为 null 的值类型
ms.date: 07/20/2015
f1_keywords:
- vb.Nullable
helpviewer_keywords:
- nullable types [Visual Basic]
- '? [Visual Basic]'
- types [Visual Basic], nullable
- nullable types [Visual Basic]
- data types [Visual Basic], nullable
ms.assetid: 9ac3b602-6f96-4e6d-96f7-cd4e81c468a6
ms.openlocfilehash: beed8262c50dc68330b8f03aa3d864ed2f8df0d5
ms.sourcegitcommit: 99b153b93bf94d0fecf7c7bcecb58ac424dfa47c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80249677"
---
# <a name="nullable-value-types-visual-basic"></a>可以为 Null 的值类型 (Visual Basic)

有时，使用在某些情况下没有定义值的值类型。 例如，数据库中的字段可能必须区分具有有意义的赋值和没有赋值。 可以扩展值类型以获取其正常值或空值。 这种扩展称为*空类型*。

每个空值类型都是从泛型<xref:System.Nullable%601>结构构造的。 考虑跟踪与工作相关的活动的数据库。 下面的示例构造一个可`Boolean`null 的类型，并声明该类型的变量。 您可以通过三种方式编写声明：

[!code-vb[VbVbalrNullableValue#1](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#1)]

变量`ridesBusToWork`可以保存`True`值 ，`False`或根本不值。 其初始默认值根本没有值，在这种情况下，这可能意味着尚未为此人获取信息。 相反，`False`可能意味着信息已经获得，并且此人不坐公共汽车去上班。

可以使用空值类型声明变量和属性，也可以声明具有空值类型的元素的数组。 您可以将具有空值类型的过程声明为参数，也可以从`Function`过程返回空值类型。

不能在引用类型（如数组、a`String`或 类）上构造可 null 类型。 基础类型必须是值类型。 有关详细信息，请参阅[值类型和参考类型](value-types-and-reference-types.md)。

## <a name="using-a-nullable-type-variable"></a>使用可空类型变量

空值类型最重要的成员是其<xref:System.Nullable%601.HasValue%2A>和<xref:System.Nullable%601.Value%2A>属性。 对于空值类型的变量，<xref:System.Nullable%601.HasValue%2A>告诉您该变量是否包含定义的值。 如果是<xref:System.Nullable%601.HasValue%2A>`True`，则可以从<xref:System.Nullable%601.Value%2A>读取 值。 请注意，两<xref:System.Nullable%601.HasValue%2A>者<xref:System.Nullable%601.Value%2A>都是`ReadOnly`属性。

### <a name="default-values"></a>默认值

当您声明具有空值类型的变量时，其<xref:System.Nullable%601.HasValue%2A>属性的默认值为`False`。 这意味着默认情况下，变量没有定义的值，而不是其基础值类型的默认值。 在下面的示例中，变量`numberOfChildren`最初没有定义的值，即使`Integer`类型的默认值为 0。

[!code-vb[VbVbalrNullableValue#2](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#2)]

空值可用于指示未定义或未知值。 如果`numberOfChildren`已声明为`Integer`，则没有任何值可以指示信息当前不可用。

### <a name="storing-values"></a>存储值

以典型方式将值存储在可 null 值类型的变量或属性中。 下面的示例将值分配给上一个示例中声明`numberOfChildren`的变量。

[!code-vb[VbVbalrNullableValue#3](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#3)]

如果空值类型的变量或属性包含已定义的值，则可能导致它恢复到未分配值的初始状态。 为此，通过将变量或属性设置为`Nothing`（）来执行此操作，如下例所示。

[!code-vb[VbVbalrNullableValue#4](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#4)]

> [!NOTE]
> 尽管可以分配给`Nothing`null 值类型的变量，但不能`Nothing`使用等号来测试它。 使用等号`someVar = Nothing`的比较， 始终计算为`Nothing`。 可以使用<xref:System.Nullable%601.HasValue%2A>或`False``Is``IsNot`运算符测试变量的属性， 或测试。

### <a name="retrieving-values"></a>检索值

若要检索 null 值类型的变量的值，应首先测试其<xref:System.Nullable%601.HasValue%2A>属性以确认其具有值。 如果尝试读取值时<xref:System.Nullable%601.HasValue%2A>为`False`，Visual Basic 将引发异常<xref:System.InvalidOperationException>。 下面的示例显示了读取前面示例变量`numberOfChildren`的建议方法。

[!code-vb[VbVbalrNullableValue#5](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#5)]

## <a name="comparing-nullable-types"></a>比较可消除类型

当布尔表达式`Boolean`中使用空变量时，结果可以是`True`，`False`或`Nothing`。 下面是`And`和`Or`的真情况表。 因为`b1`现在有`b2`三个可能的值，因此有九个组合需要计算。

|b1|b2|b1 和 b2|b1 或 b2|
|--------|--------|---------------|--------------|
|`Nothing`|`Nothing`|`Nothing`|`Nothing`|
|`Nothing`|`True`|`Nothing`|`True`|
|`Nothing`|`False`|`False`|`Nothing`|
|`True`|`Nothing`|`Nothing`|`True`|
|`True`|`True`|`True`|`True`|
|`True`|`False`|`False`|`True`|
|`False`|`Nothing`|`False`|`Nothing`|
|`False`|`True`|`False`|`True`|
|`False`|`False`|`False`|`False`|

当布尔变量或表达式的值为`Nothing`时，它既不是 ，`true`也不是`false`。 请考虑以下示例。

[!code-vb[VbVbalrNullableValue#6](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#6)]

在此示例中，`b1 And b2`计算 到`Nothing`。 因此，子`Else`句在每个`If`语句中执行，输出如下所示：

`Expression is not true`

`Expression is not false`

> [!NOTE]
> `AndAlso`和`OrElse`，使用短路评估时，必须评估其第二个操作数， 当第一个`Nothing`计算到 。

## <a name="propagation"></a>传播

如果算术、比较、移位或类型操作的一个或两个操作数是空值类型，则操作的结果也是空值类型。 如果两个操作数的值都不是`Nothing`，则对操作数的基础值执行操作，就像两者都不是空值类型一样。 在下面的示例中，变量`compare1`和`sum1`隐式类型。 如果将鼠标指针放在鼠标指针上，您将看到编译器推断它们两个指针的空值类型。

[!code-vb[VbVbalrNullableValue#7](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#7)]

如果一个或两个操作数的值`Nothing`为 ， 则结果将为`Nothing`。

[!code-vb[VbVbalrNullableValue#8](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#8)]

## <a name="using-nullable-types-with-data"></a>将可空类型与数据一起使用

数据库是使用空值类型的最重要位置之一。 并非所有数据库对象当前都支持空值类型，但设计器生成的表适配器支持。 有关[空类型的表适配器支持，请参阅表适配器支持](/visualstudio/data-tools/fill-datasets-by-using-tableadapters#tableadapter-support-for-nullable-types)。

## <a name="see-also"></a>请参阅

- <xref:System.InvalidOperationException>
- <xref:System.Nullable%601.HasValue%2A>
- [数据类型](index.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [使用 Tableadapter 填充数据集](/visualstudio/data-tools/fill-datasets-by-using-tableadapters)
- [If 运算符](../../../language-reference/operators/if-operator.md)
- [局部类型推理](../variables/local-type-inference.md)
- [Is 运算符](../../../language-reference/operators/is-operator.md)
- [IsNot 运算符](../../../language-reference/operators/isnot-operator.md)
- [空值类型 （C#）](../../../../csharp/language-reference/builtin-types/nullable-value-types.md)
