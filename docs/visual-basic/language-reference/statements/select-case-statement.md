---
title: Select...Case 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Select
- vb.Case
helpviewer_keywords:
- Select statement [Visual Basic]
- Case statement [Visual Basic]
- Select...Case statements
- conditional statements [Visual Basic], Select Case
- control flow [Visual Basic], branching
- Else keyword [Visual Basic], in Select...Case statements
- execution [Visual Basic], conditional
- To keyword [Visual Basic], in Select...Case statements
- Select Case statement [Visual Basic], Select...Case
- Select statement [Visual Basic], Select...Case
- Is operator [Visual Basic], in Select...Case statements
- branching [Visual Basic], conditional
- Case Else statement [Visual Basic], Select...Case
- End keyword [Visual Basic], Select Case statements
- Case statement [Visual Basic], Select...Case
ms.assetid: 68877b65-5419-4bf0-a465-20cd0e4c7d44
ms.openlocfilehash: 750e765390ad223976b000fe64e656fa2d62a34b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90871785"
---
# <a name="selectcase-statement-visual-basic"></a>Select...Case 语句 (Visual Basic)

根据表达式的值运行若干组语句中的一个。  
  
## <a name="syntax"></a>语法  
  
```vb  
Select [ Case ] testexpression  
    [ Case expressionlist  
        [ statements ] ]  
    [ Case Else  
        [ elsestatements ] ]  
End Select  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`testexpression`|必需。 表达式。 的计算结果必须为其中一种基本数据类型 (、、、、、、、、、、、、、、 `Boolean` `Byte` `Char` `Date` `Double` `Decimal` `Integer` `Long` `Object` `SByte` `Short` `Single` `String` `UInteger` `ULong` 和 `UShort`) 。|  
|`expressionlist`|在语句中是必需的 `Case` 。 表示的匹配值的表达式子句的列表 `testexpression` 。 多个表达式子句之间用逗号分隔。 每个子句可以采用以下形式之一：<br /><br /> -   *表达式* `To` =*表达式*2<br />-[ `Is` ] *.comparisonoperator* *表达式*<br />-   *表达式*<br /><br /> 使用 `To` 关键字可以指定的匹配值范围的边界 `testexpression` 。 的值 `expression1` 必须小于或等于的值 `expression2` 。<br /><br /> 使用 `Is` 关键字和比较运算符 (`=` 、 `<>` 、、、 `<` `<=` `>` 或 `>=`) 来指定对的匹配值的限制 `testexpression` 。 如果 `Is` 未提供关键字，则自动将其插入 *.comparisonoperator*之前。<br /><br /> 仅指定的窗体 `expression` 被视为窗体的一种特殊情况， `Is` 其中 *.comparisonoperator* 是) 的等号 (`=` 。 此形式的计算结果为 `testexpression`  =  `expression` 。<br /><br /> 中的表达式 `expressionlist` 可以是任何数据类型，前提是它们可隐式转换为的类型 `testexpression` ，并且相应的 `comparisonoperator` 对于与之一起使用的两种类型都有效。|  
|`statements`|可选。 `Case`如果 `testexpression` 与中的任何子句匹配，则为运行的一个或多个语句 `expressionlist` 。|  
|`elsestatements`|可选。 `Case Else`如果不 `testexpression` 与任何语句的中的任何子句匹配，则为运行的一个或多个语句 `expressionlist` `Case` 。|  
|`End Select`|终止 `Select` ... 构造的定义。 `Case`|  
  
## <a name="remarks"></a>备注  

 如果 `testexpression` 与 any `Case` `expressionlist` 子句匹配，则该语句后面的语句将 `Case` 运行到下一个 `Case` 、 `Case Else` 或 `End Select` 语句。 然后，控制将传递到后面的语句 `End Select` 。 如果 `testexpression` `expressionlist` 在多个子句中匹配某个子句 `Case` ，则只运行第一个匹配后的语句。  
  
 `Case Else` `elsestatements` 如果在 `testexpression` `expressionlist` 任何其他语句中的和子句之间找不到匹配项，则该语句用于引入要运行的 `Case` 。 尽管这不是必需的，但最好 `Case Else` 在构造中具有语句 `Select Case` 来处理不可预见的 `testexpression` 值。 如果没有任何 `Case` `expressionlist` 子句匹配 `testexpression` 并且没有 `Case Else` 语句，控制将传递到后面的语句 `End Select` 。  
  
 可以在每个子句中使用多个表达式或范围 `Case` 。 例如，下面的行是有效的。  
  
 `Case 1 To 4, 7 To 9, 11, 13, Is > maxNumber`  
  
> [!NOTE]
> `Is`和语句中使用的关键字与 `Case` `Case Else` [is 运算符](../operators/is-operator.md)不同，后者用于对象引用比较。  
  
 您可以为字符串指定范围和多个表达式。 在下面的示例中， `Case` 匹配任何完全等于 "苹果" 的字符串，其值介于 "螺母" 和 "soup" 之间的字母顺序之间，或包含与当前值完全相同的值 `testItem` 。  
  
 `Case "apples", "nuts" To "soup", testItem`  
  
 的设置 `Option Compare` 可能会影响字符串比较。 在下 `Option Compare Text` ，字符串 "苹果" 和 "苹果" 比较视为相等，但在下， `Option Compare Binary` 它们不会。  
  
> [!NOTE]
> `Case`具有多个子句的语句可以表现称为 "*短路*" 的行为。 Visual Basic 从左到右计算子句的值，并且如果一个语句生成匹配项 `testexpression` ，则不会计算剩余的子句。 短路可以提高性能，但如果希望计算中的每个表达式，则可能产生意外的结果 `expressionlist` 。 有关短路的详细信息，请参阅 [布尔表达式](../../programming-guide/language-features/operators-and-expressions/boolean-expressions.md)。  
  
 如果 `Case` 或语句块内的代码 `Case Else` 不需要在块中运行任何更多语句，则可以使用语句退出块 `Exit Select` 。 这会将控制立即传输到后面的语句 `End Select` 。  
  
 `Select Case` 构造可以嵌套。 每个嵌套 `Select Case` 构造都必须具有匹配的 `End Select` 语句，并且必须完全包含在 `Case` `Case Else` 嵌套它的外部构造的单个或语句块内 `Select Case` 。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select Case` 构造来编写与变量的值相对应的行 `number` 。 第二个 `Case` 语句包含的值与的当前值匹配 `number` ，因此将运行写入 "6 到8个（含）" 的语句。  
  
 [!code-vb[VbVbalrStatements#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#54)]  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Interaction.Choose%2A>
- [End 语句](end-statement.md)
- [If...Then...Else 语句](if-then-else-statement.md)
- [Option Compare 语句](option-compare-statement.md)
- [Exit 语句](exit-statement.md)
