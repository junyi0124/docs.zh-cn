---
title: = 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.Assign
- vb.=
helpviewer_keywords:
- = operator [Visual Basic]
- = assignment statements [Visual Basic]
ms.assetid: 2dac2e49-86c8-42f8-80c1-458452fb5e29
ms.openlocfilehash: eccea0b43564a4980778c9d1a5b8f9a8c2a9207d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874820"
---
# <a name="-operator-visual-basic"></a>= 运算符 (Visual Basic)

为变量或属性赋值。  
  
## <a name="syntax"></a>语法  
  
```vb  
variableorproperty = value  
```  
  
## <a name="parts"></a>组成部分  

 `variableorproperty`  
 任何可写的变量或任何属性。  
  
 `value`  
 任何文本、常量或表达式。  
  
## <a name="remarks"></a>备注  

 等于符号 () 左侧的元素 `=` 可以是简单的标量变量、属性或数组元素。 变量或属性不能是 [只读](../modifiers/readonly.md)的。 `=`运算符将其右侧的值赋值给其左侧的变量或属性。  
  
> [!NOTE]
> `=`运算符也用作比较运算符。 有关详细信息，请参阅 [比较运算符](comparison-operators.md)。  
  
## <a name="overloading"></a>重载  

 `=`运算符只能作为关系比较运算符重载，而不能作为赋值运算符重载。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示了赋值运算符。 右侧的值将分配给左侧的变量。  
  
 [!code-vb[VbVbalrOperators#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>另请参阅

- [&= 运算符](and-assignment-operator.md)
- [* = 运算符](multiplication-assignment-operator.md)
- [+ = 运算符](addition-assignment-operator.md)
- [-= 运算符 (Visual Basic) ](subtraction-assignment-operator.md)
- [/= 运算符 (Visual Basic) ](floating-point-division-assignment-operator.md)
- [\\= 运算符](integer-division-assignment-operator.md)
- [^ = 运算符](exponentiation-assignment-operator.md)
- [语句](../../programming-guide/language-features/statements.md)
- [比较运算符](comparison-operators.md)
- [ReadOnly](../modifiers/readonly.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
