---
title: 比较运算符
ms.date: 07/20/2015
f1_keywords:
- vb.<>
- vb.>=
- vb.<=
- vb.>
- vb.<
helpviewer_keywords:
- greater than or equal to operator [Visual Basic]
- '>= operator [Visual Basic]'
- = operator [Visual Basic]
- < operator [Visual Basic]
- less than operator [Visual Basic]
- relational operators [Visual Basic], syntax
- Like operator [Visual Basic]
- <> operator [Visual Basic]
- '> operator [Visual Basic]'
- equal operator [Visual Basic]
- less than or equal to operator [Visual Basic]
- symbols, operators
- greater than operator [Visual Basic]
- comparing values [Visual Basic]
- operators [Visual Basic], relational
- string comparison [Visual Basic]
- not equal to comparison operator [Visual Basic]
- <= operator [Visual Basic]
- operators [Visual Basic], comparison
- Is operator [Visual Basic]
- comparison operators [Visual Basic], Visual Basic
ms.assetid: d6cb12a8-e52e-46a7-8aaf-f804d634a825
ms.openlocfilehash: fcbb9052a79fa4b20b5a0f8fdc15de73d55a4281
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873440"
---
# <a name="comparison-operators-visual-basic"></a>比较运算符 (Visual Basic)

下面是 Visual Basic 中定义的比较运算符。

 `<` 操作员

 `<=` 操作员

 `>` 操作员

 `>=` 操作员

 `=` 操作员

 `<>` 操作员

 [Is 运算符](is-operator.md)

 [IsNot 运算符](isnot-operator.md)

 [Like 运算符](like-operator.md)

 这些运算符比较两个表达式以确定它们是否相等，如果不相等，则确定它们之间的差异。 `Is``IsNot` `Like` 在单独的帮助页上详细讨论了、和。 此页上详细讨论了关系比较运算符。

## <a name="syntax"></a>语法
  
```vb
result = expression1 comparisonoperator expression2  
result = object1 [Is | IsNot] object2  
result = string Like pattern  
```  
  
## <a name="parts"></a>组成部分

 `result`  
 必需。 一个 `Boolean` 值，该值表示比较的结果。

 `expression1`, `expression2`  
 必需。 任何表达式。

 `comparisonoperator`  
 必需。 任何关系比较运算符。

 `object1`, `object2`  
 必需。 任何引用对象的名称。

 `string`  
 必需。 任何 `String` 表达式。

 `pattern`  
 必需。 任何 `String` 表达式或字符范围。

## <a name="remarks"></a>备注

 下表包含关系比较运算符和确定是还是的条件的列表 `result` `True` `False` 。

|运算符|`True` 是否|`False` 是否|
|--------------|---------------|----------------|
|`<` (小于) |`expression1` < `expression2`|`expression1` >= `expression2`|
|`<=` (小于或等于) |`expression1` <= `expression2`|`expression1` > `expression2`|
|`>` (大于) |`expression1` > `expression2`|`expression1` <= `expression2`|
|`>=` (大于或等于) |`expression1` >= `expression2`|`expression1` < `expression2`|
|`=` (等于) |`expression1` = `expression2`|`expression1` <> `expression2`|
|`<>` (不等于) |`expression1` <> `expression2`|`expression1` = `expression2`|

> [!NOTE]
> [= 运算符](assignment-operator.md)也用作赋值运算符。

 `Is`运算符、运算符 `IsNot` 和 `Like` 运算符具有特定的比较功能，这些功能不同于上表中的运算符。

## <a name="comparing-numbers"></a>比较数字

 将类型的表达式与类型的表达式进行比较时 `Single` `Double` ， `Single` 表达式将转换为 `Double` 。 此行为与 Visual Basic 6 中的行为相反。

 同样，将类型的表达式与 `Decimal` 或类型的表达式进行比较时 `Single` `Double` ， `Decimal` 表达式将转换为 `Single` 或 `Double` 。 对于 `Decimal` 表达式，小于 1e-28 的任何小数值都可能会丢失。 此类小数值损失可能导致两个值在不时比较为相等。 出于此原因，在使用相等 (`=`) 比较两个浮点变量时，应格外小心。 更安全的做法是测试两个数字之差的绝对值是否小于可接受的小容差。

### <a name="floating-point-imprecision"></a>浮点不精确性

 使用浮点数时，请记住，它们在内存中不一定有精确的表示形式。 这可能会导致某些操作产生意外结果，如值比较和 [Mod 运算符](mod-operator.md)。 有关详细信息，请参阅 [数据类型疑难解答](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)。

## <a name="comparing-strings"></a>比较字符串

 在比较字符串时，将根据设置的字母顺序对字符串表达式进行计算 `Option Compare` 。

 `Option Compare Binary` 对从字符的内部二进制表示形式派生的排序顺序进行字符串比较。 排序顺序由代码页确定。 下面的示例显示了一个典型的二进制排序顺序。

 `A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`

 `Option Compare Text` 基于不区分大小写的文本排序顺序对应用程序的区域设置进行字符串比较。 设置 `Option Compare Text` 和排序前面的示例中的字符时，将应用以下文本排序顺序：

 `(A=a) < (À= à) < (B=b) < (E=e) < (Ê= ê) < (Ø = ø) < (Z=z)`

### <a name="locale-dependence"></a>区域设置依赖关系

 设置时 `Option Compare Text` ，字符串比较的结果可能取决于应用程序运行所在的区域设置。 在一个区域设置中，两个字符的比较结果可能不相同，而在另一个区域中 如果使用字符串比较来做出重要决策（例如是否接受登录尝试），则应注意区域设置的敏感性。 请考虑设置 `Option Compare Binary` 或调用 <xref:Microsoft.VisualBasic.Strings.StrComp%2A> ，这会使区域设置生效。

## <a name="typeless-programming-with-relational-comparison-operators"></a>用关系比较运算符进行无方式编程

 不允许在下使用带有表达式的关系比较运算符 `Object` `Option Strict On` 。 如果 `Option Strict` 为 `Off` ，并且 `expression1` 或 `expression2` 是 `Object` 表达式，则运行时类型确定如何比较它们。 下表显示了如何比较表达式和比较的结果，具体取决于操作数的运行时类型。

|如果操作数为|比较是|
|---------------------|-------------------|
|全部 `String`|基于字符串排序特性的排序比较。|
|均为数值|转换为 `Double` 数值比较的对象。|
|一个数字和一个 `String`|`String`转换为 `Double` 并执行数值比较。 如果 `String` 无法转换为 `Double` ， <xref:System.InvalidCastException> 则会引发。|
|其中一个或两个都是引用类型，而不是 `String`|引发 <xref:System.InvalidCastException>。|

 数值比较视为 `Nothing` 0。 字符串比较视为 `Nothing` `""` (空字符串) 。

## <a name="overloading"></a>重载

 关系比较运算符 (`<` 。 `<=`、 `>` 、 `>=` 、 `=` 、 `<>`) 可以进行 *重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义它们的行为。 如果你的代码在此类或结构上使用这些运算符中的任何一种，请确保了解重定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。

 请注意， [= 运算符](assignment-operator.md) 只能作为关系比较运算符重载，而不能作为赋值运算符重载。

## <a name="example"></a>示例

 下面的示例演示了用于比较表达式的关系比较运算符的各种用法。 关系比较运算符返回一个 `Boolean` 结果，该结果表示指定表达式的计算结果是否为 `True` 。 将 `>` and `<` 运算符应用到字符串时，将使用标准的按字母顺序排序的字符串进行比较。 此顺序可能取决于区域设置。 排序是否区分大小写取决于 " [选项比较](../statements/option-compare-statement.md) " 设置。

 [!code-vb[VbVbalrOperators#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#1)]

 在前面的示例中，第一个比较返回， `False` 其余比较返回 `True` 。

## <a name="see-also"></a>另请参阅

- <xref:System.InvalidCastException>
- [= 运算符](assignment-operator.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [数据类型疑难解答](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Comparison Operators in Visual Basic](../../programming-guide/language-features/operators-and-expressions/comparison-operators.md)
