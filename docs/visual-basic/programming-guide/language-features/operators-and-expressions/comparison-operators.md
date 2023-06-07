---
title: 比较运算符
ms.date: 07/20/2015
helpviewer_keywords:
- comparison operators [Visual Basic], comparing strings
- comparison operators [Visual Basic], comparing objects
- strings [Visual Basic], comparing
- comparison operators [Visual Basic]
- string comparison [Visual Basic], operators
- objects [Visual Basic], comparing
- numbers [Visual Basic], comparing
- Visual Basic code, operators
- string comparison [Visual Basic]
- numeric values [Visual Basic], comparing
- comparison operators [Visual Basic], comparing numeric values
- operators [Visual Basic], comparison
ms.assetid: 0b570339-5407-474f-8421-e183a8b303ee
ms.openlocfilehash: fbe81532bb435e54e694f9b5fe9dd497392f31e1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071763"
---
# <a name="comparison-operators-in-visual-basic"></a>Comparison Operators in Visual Basic

比较运算符比较两个表达式，并返回一个 `Boolean` 值，该值表示其值之间的关系。 有一些运算符用于比较数值、用于比较字符串的运算符以及用于比较对象的运算符。 这里讨论了所有这三种类型的运算符。  
  
## <a name="comparing-numeric-values"></a>比较数值  

 Visual Basic 使用六个数值比较运算符比较数值。 每个运算符都采用两个计算为数值的表达式作为操作数。 下表列出了运算符，并显示了每个运算符的示例。  
  
|运算符|测试的条件|示例|  
|--------------|----------------------|--------------|  
|`=` (相等) |第一个表达式的值是否等于第二个表达式的值？|`23`   `=`   `33    ' False`<br /><br /> `23`   `=`   `23    ' True`<br /><br /> `23`   `=`   `12    ' False`|  
|`<>` (不相等) |第一个表达式的值是否与第二个表达式的值不相等？|`23`   `<>`   `33    ' True`<br /><br /> `23`   `<>`   `23    ' False`<br /><br /> `23`   `<>`   `12    ' True`|  
|`<` (小于) |第一个表达式的值是否小于第二个表达式的值？|`23`   `<`   `33    ' True`<br /><br /> `23`   `<`   `23    ' False`<br /><br /> `23`   `<`   `12    ' False`|  
|`>` (大于) |第一个表达式的值是否大于第二个表达式的值？|`23`   `>`   `33    ' False`<br /><br /> `23`   `>`   `23    ' False`<br /><br /> `23`   `>`   `12    ' True`|  
|`<=` (小于或等于) |第一个表达式的值是否小于或等于第二个表达式的值？|`23`   `<=`   `33    ' True`<br /><br /> `23`   `<=`   `23    ' True`<br /><br /> `23`   `<=`   `12    ' False`|  
|`>=` (大于或等于) |第一个表达式的值是否大于或等于第二个表达式的值？|`23`   `>=`   `33    ' False`<br /><br /> `23`   `>=`   `23    ' True`<br /><br /> `23`   `>=`   `12    ' True`|  
  
## <a name="comparing-strings"></a>比较字符串  

 Visual Basic 使用 [Like 运算符](../../../language-reference/operators/like-operator.md) 以及数字比较运算符来比较字符串。 `Like`运算符用于指定模式。 然后，将该字符串与模式进行比较，如果匹配，则结果为 `True` 。 否则，结果为 `False`。 数字运算符允许您根据值的 `String` 排序顺序比较值，如下面的示例所示。  
  
 `"73" < "9"`  
  
 `' The result of the preceding comparison is True.`  
  
 前面的示例中的结果是， `True` 因为第一个字符串中的第一个字符在第二个字符串中的第一个字符之前排序。 如果第一个字符相等，则比较将继续执行两个字符串中的下一个字符，依此类推。 你还可以使用相等运算符测试字符串的相等性，如下面的示例所示。  
  
 `"734" = "734"`  
  
 `' The result of the preceding comparison is True.`  
  
 如果一个字符串是另一个字符串的前缀（如 "aa" 和 "aaa"），则较长的字符串将被视为大于较短的字符串。 下面的示例对此进行了演示。  
  
 `"aaa" > "aa"`  
  
 `' The result of the preceding comparison is True.`  
  
 根据的设置，排序顺序基于二进制比较或文本比较 `Option Compare` 。 有关详细信息，请参阅 [Option Compare 语句](../../../language-reference/statements/option-compare-statement.md)。  
  
## <a name="comparing-objects"></a>比较对象  

 Visual Basic 将两个对象引用变量与 [Is 运算符](../../../language-reference/operators/is-operator.md) 和 [IsNot 运算符](../../../language-reference/operators/isnot-operator.md)进行比较。 您可以使用其中任一运算符来确定两个引用变量是否引用同一对象实例。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbalrOperators#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#65)]  
  
 在前面的示例中， `x Is y` 计算结果为 `True` ，因为这两个变量引用相同的实例。 将此结果与下面的示例进行比较。  
  
 [!code-vb[VbVbalrOperators#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#66)]  
  
 在前面的示例中， `x Is y` 计算结果为 `False` ，因为尽管变量引用相同类型的对象，但它们引用该类型的不同实例。  
  
 如果要测试两个对象是否不指向同一个实例， `IsNot` 运算符使你可以避免与的语法笨拙组合 `Not` `Is` 。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbalrOperators#67](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#67)]  
  
 在前面的示例中， `If a IsNot b` 等效于 `If Not a Is b` 。  
  
### <a name="comparing-object-type"></a>比较对象类型  

 您可以使用 `TypeOf` ... 表达式测试对象是否为特定类型。 `Is` 语法如下：  
  
 `TypeOf <objectexpression> Is <typename>`  
  
 当 `typename` 指定接口类型时， `TypeOf` `Is` `True` 如果对象实现接口类型，则 ... 表达式将返回。 当 `typename` 是类类型时， `True` 如果对象是指定类的实例或从指定类派生的类，则表达式返回。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbalrOperators#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#68)]  
  
 在前面的示例中， `TypeOf x Is Control` 表达式的计算结果为 `True` ，因为的类型 `x` 为 `Button` ，后者继承自 `Control` 。  
  
 有关详细信息，请参阅 [TypeOf 运算符](../../../language-reference/operators/typeof-operator.md)。  
  
## <a name="see-also"></a>请参阅

- [值的比较](value-comparisons.md)
- [比较运算符](../../../language-reference/operators/comparison-operators.md)
- [运算符](../../../language-reference/operators/index.md)
- [算术运算符 (Visual Basic)](arithmetic-operators.md)
- [串联运算符 (Visual Basic)](concatenation-operators.md)
- [Visual Basic 中的逻辑运算符和位运算符](logical-and-bitwise-operators.md)
