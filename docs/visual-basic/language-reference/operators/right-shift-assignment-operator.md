---
title: '>>= 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.>>=
helpviewer_keywords:
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- operator >>= [Visual Basic]
- compound assignment statements [Visual Basic]
- '>>= operator [Visual Basic]'
ms.assetid: 2bcd9abb-7a8c-4229-b75d-8816ff1dc700
ms.openlocfilehash: a1c2331791644155504183d3cf5767470a3bf17b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873357"
---
# <a name="-operator-visual-basic"></a>>>= 运算符 (Visual Basic) 

对变量或属性的值执行算术右移位，并将结果赋回给变量或属性。  
  
## <a name="syntax"></a>语法  
  
```vb  
variableorproperty >>= amount  
```  
  
## <a name="parts"></a>组成部分  

 `variableorproperty`  
 必需。 整数类型的变量或属性 (`SByte` 、、、、、、 `Byte` `Short` `UShort` `Integer` `UInteger` `Long` 或 `ULong`) 。  
  
 `amount`  
 必需。 扩大到的数据类型的数值表达式 `Integer` 。  
  
## <a name="remarks"></a>备注  

 运算符左侧的元素 `>>=` 可以是简单的标量变量、属性或数组的元素。 变量或属性不能是 [只读](../modifiers/readonly.md)的。  
  
 `>>=`运算符首先对变量或属性的值执行算术右移位运算。 然后，运算符将该操作的结果赋给变量或属性。  
  
 算术移位不是循环的，这意味着，不会在另一端重新引入结果的末尾以外的位。 在算术右移位时，将丢弃超出最右位位置的位，并将最左侧的位传播到左端空出的位位置。 这意味着，如果 `variableorproperty` 具有负值，空出位置将设置为1。 如果 `variableorproperty` 为正，或者其数据类型为无符号类型，则空出位置将设置为零。  
  
## <a name="overloading"></a>重载  

 可以*重载* [>> 运算符](right-shift-operator.md)，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 重载 `>>` 运算符会影响运算符的行为 `>>=` 。 如果你的代码 `>>=` 在重载的类或结构上使用 `>>` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `>>=` 运算符将变量的位模式右移指定的 `Integer` 量，并将结果赋给该变量。  
  
 [!code-vb[VbVbalrOperators#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>另请参阅

- [>> 运算符](right-shift-operator.md)
- [赋值运算符](assignment-operators.md)
- [移位运算符](bit-shift-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [语句](../../programming-guide/language-features/statements.md)
