---
title: /= 运算符
ms.date: 07/20/2015
f1_keywords:
- vb./=
helpviewer_keywords:
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- /= operator [Visual Basic]
- operator /=
- compound assignment statements [Visual Basic]
ms.assetid: a1e22d0e-8380-4761-9da1-84fb51c34821
ms.openlocfilehash: d47a69e454305ce9417a46b5bbfbbb55a1ad1dc3
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867080"
---
# <a name="-operator-visual-basic"></a>/= 运算符 (Visual Basic)

将变量或属性的值除以表达式的值，并将浮点结果赋给变量或属性。  
  
## <a name="syntax"></a>语法  
  
```vb  
variableorproperty /= expression  
```  
  
## <a name="parts"></a>组成部分  

 `variableorproperty`  
 必需。 任何数值变量或属性。  
  
 `expression`  
 必需。 任何数值表达式。  
  
## <a name="remarks"></a>备注  

 运算符左侧的元素 `/=` 可以是简单的标量变量、属性或数组的元素。 变量或属性不能是 [只读](../modifiers/readonly.md)的。  
  
 `/=`运算符首先将运算符左侧的变量或属性的值除以运算符) 右侧 (表达式的值来)  (的变量或属性值。 然后，运算符将该操作的浮点结果分配给变量或属性。  
  
 此语句将值赋 `Double` 给左侧的变量或属性。 如果 `Option Strict` 为 `On` ，则 `variableorproperty` 必须为 `Double` 。 如果 `Option Strict` 为 `Off` ，Visual Basic 将执行隐式转换，并将生成的值分配给 `variableorproperty` ，并在运行时出现错误。 有关详细信息，请参阅 [扩大和收缩转换](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md) 和 [Option Strict 语句](../statements/option-strict-statement.md)。  
  
## <a name="overloading"></a>重载  

 [/运算符 (Visual Basic) ](floating-point-division-operator.md)可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 重载 `/` 运算符会影响运算符的行为 `/=` 。 如果你的代码 `/=` 在重载的类或结构上使用 `/` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `/=` 运算符将一个变量除以 `Integer` 第二个变量，并将商赋给第一个变量。  
  
 [!code-vb[VbVbalrOperators#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>另请参阅

- [/Operator (Visual Basic) ](floating-point-division-operator.md)
- [\\= 运算符](integer-division-assignment-operator.md)
- [赋值运算符](assignment-operators.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [语句](../../programming-guide/language-features/statements.md)
