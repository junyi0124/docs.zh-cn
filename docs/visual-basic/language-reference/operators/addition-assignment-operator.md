---
title: += 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.+=
helpviewer_keywords:
- += operator [Visual Basic]
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- += operator [Visual Basic], appending strings
- compound assignment statements [Visual Basic]
ms.assetid: d3e959f4-85d4-4e47-87c4-77b62335a5b3
ms.openlocfilehash: a3a37798a3ddb480ac5322c4b2d3e9396e739aa6
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873480"
---
# <a name="-operator-visual-basic"></a>+= 运算符 (Visual Basic)

将数值表达式的值添加到数值变量或属性的值，并将结果赋给变量或属性。 还可用于将 `String` 表达式连接到 `String` 变量或属性，并将结果赋给变量或属性。  
  
## <a name="syntax"></a>语法  
  
```vb  
variableorproperty += expression  
```  
  
## <a name="parts"></a>组成部分  

 `variableorproperty`  
 必需。 任何数值或 `String` 变量或属性。  
  
 `expression`  
 必需。 任何数值或 `String` 表达式。  
  
## <a name="remarks"></a>备注  

 运算符左侧的元素 `+=` 可以是简单的标量变量、属性或数组的元素。 变量或属性不能是 [只读](../modifiers/readonly.md)的。  
  
 `+=`运算符将其右侧的值添加到其左侧的变量或属性，并将结果赋给其左侧的变量或属性。 `+=`运算符还可用于将 `String` 其右侧的表达式连接到 `String` 左侧的变量或属性，并将结果分配给其左侧的变量或属性。  
  
> [!NOTE]
> 使用 `+=` 运算符时，可能无法确定是进行加法还是字符串串联。 使用 `&=` 运算符进行串联以消除多义性，并提供自文档代码。  
  
 如果编译环境强制执行严格语义，则此赋值运算符隐式执行扩大但不进行收缩转换。 有关这些转换的详细信息，请参阅 [扩大和收缩转换](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)。 有关严格语义和许可语义的详细信息，请参阅 [Option Strict 语句](../statements/option-strict-statement.md)。  
  
 如果允许使用允许的 `+=` 隐式语义，运算符将隐式执行与运算符执行的操作相同的各种字符串和数值转换 `+` 。 有关这些转换的详细信息，请参阅 [+ 运算符](addition-operator.md)。  
  
## <a name="overloading"></a>重载  

 `+`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 重载 `+` 运算符会影响运算符的行为 `+=` 。 如果你的代码 `+=` 在重载的类或结构上使用 `+` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `+=` 运算符将一个变量的值与另一个变量组合在一起。 第一部分使用 `+=` 数值变量将一个值添加到另一个值。 第二部分使用 `+=` with `String` 变量将一个值与另一个值连接起来。 在这两种情况下，都会将结果赋给第一个变量。  
  
 [!code-vb[VbVbalrOperators#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#7)]  
  
 [!code-vb[VbVbalrOperators#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#8)]  
  
 的值 `num1` 现在为13，的值 `str1` 现在为 "103"。  
  
## <a name="see-also"></a>另请参阅

- [+ 运算符](addition-operator.md)
- [赋值运算符](assignment-operators.md)
- [算术运算符](arithmetic-operators.md)
- [串联运算符](concatenation-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [语句](../../programming-guide/language-features/statements.md)
