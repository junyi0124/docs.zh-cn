---
title: 代码中的特殊字符
ms.date: 07/20/2015
f1_keywords:
- vb.)
- vb.(
- vb.colon
- vb.!
- vb..
- 'vb.:'
helpviewer_keywords:
- special characters [Visual Basic], in code
- parentheses [Visual Basic], using in code
- colons (:)
- period character in code
- dot operator (.)
- dictionary access operator [Visual Basic]
- concatenation operators [Visual Basic], special characters in code
- concatenation operators [Visual Basic], vs. addition operator
- '! operator'
- separators [Visual Basic], using in code
- operators [Visual Basic], dictionary access
- ': separator character'
- member access operator [Visual Basic]
- addition operator [Visual Basic]
- operators [Visual Basic], member access
- . operator
- exclamation points
- separators
- exclamation point operator (!)
- Visual Basic code, special characters
ms.assetid: 310dce0c-45b5-4e0d-83e9-32df258d2a3e
ms.openlocfilehash: 60f815f0d30fa785f4a2166db5a041d3851aa954
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097821"
---
# <a name="special-characters-in-code-visual-basic"></a>代码中的特殊字符 (Visual Basic)

有时，您必须在代码中使用特殊字符，即不是字母或数字的字符。 Visual Basic 字符集中的标点和特殊字符具有多种用途，从组织程序文本到定义编译器或已编译程序所执行的任务。 它们不指定要执行的操作。  
  
## <a name="parentheses"></a>括号  

 定义过程（如或）时，请使用括号 `Sub` `Function` 。 必须将所有过程参数列表括在括号中。 还可以使用括号将变量或参数置于逻辑组中，特别是在复杂表达式中覆盖运算符优先级的默认顺序。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbcnConventions#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#11)]  
  
 执行前面的代码后，的值 `d` 为8.225，值为 `e` 3。 的计算 `d` 使用的默认优先级为 " `/` over" `+` ，等效于 `d = b + (c / a)` 。 计算中的括号用于 `e` 重写默认优先级。  
  
## <a name="separators"></a>分隔器  

 分隔符可执行其名称建议：它们分隔代码段。 在 Visual Basic 中，分隔符为冒号 (`:`) 。 如果要在单个行而不是单独的行中包含多个语句，请使用分隔符。 这样可以节省空间，并可提高代码的可读性。 下面的示例显示了以冒号分隔的三个语句。  
  
 [!code-vb[VbVbcnConventions#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#12)]  
  
 有关详细信息，请参阅 [如何：在代码中中断和组合语句](how-to-break-and-combine-statements-in-code.md)。  
  
 冒号 (`:`) 字符还用于标识语句标签。 有关详细信息，请参阅 [如何：标签语句](how-to-label-statements.md)。  
  
## <a name="concatenation"></a>串联  

 使用 `&` 运算符进行 *串联*，或将字符串链接在一起。 不要将其与运算符混淆 `+` ，后者将数值相加。 如果在 `+` 操作数值时使用运算符进行连接，则可以获得不正确的结果。 下面的示例演示这一操作。  
  
 [!code-vb[VbVbcnConventions#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#13)]  
  
 执行前面的代码后，的值 `resultA` 为21.01，值为 `resultB` "10.0111"。  
  
## <a name="member-access-operators"></a>成员访问运算符  

 若要访问类型的成员，请使用点 (`.`) 或感叹号 (`!` 类型名称和成员名称之间的) 运算符。  
  
### <a name="dot--operator"></a> ( ) 运算符  

 `.`在类、结构、接口或枚举上使用运算符作为成员访问运算符。 成员可以是字段、属性、事件或方法。 下面的示例对此进行了演示。  
  
 [!code-vb[VbVbcnConventions#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#14)]  
  
### <a name="exclamation-point--operator"></a>感叹号 (！ ) 运算符  

 `!`仅在类或接口上将运算符用作字典访问运算符。 类或接口必须具有接受单个参数的默认属性 `String` 。 紧跟在运算符后面的标识符 `!` 将成为作为字符串传递到默认属性的参数值。 下面的示例演示这一操作。  
  
 [!code-vb[VbVbcnConventions#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#15)]  
  
 所有的三个输出行 `MsgBox` 都显示值 `32856` 。 第一行使用对属性的传统访问 `index` ，第二行使用 `index` 作为类的默认属性的事实 `hasDefault` ，第三行使用对类的字典访问。  
  
 请注意，运算符的第二个操作数 `!` 必须是一个有效的 Visual Basic 标识符，不能用双引号引起来 (`" "`) 。 换句话说，您不能使用字符串文本或字符串变量。 对于调用的最后一行，以下更改将 `MsgBox` 生成错误，因为 `"X"` 是括起来的字符串文字。  
  
 `"Dictionary access returns " & hD!"X")`  
  
> [!NOTE]
> 对默认集合的引用必须是显式的。 特别是，不能 `!` 对后期绑定变量使用运算符。  
  
 该 `!` 字符还用作 `Single` 类型字符。  
  
## <a name="see-also"></a>请参阅

- [程序结构和代码约定](program-structure-and-code-conventions.md)
- [类型字符](../language-features/data-types/type-characters.md)
