---
title: '&amp;= 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.&=
helpviewer_keywords:
- operator &=
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- '&= operator [Visual Basic]'
- compound assignment statements [Visual Basic]
ms.assetid: 0cf262fc-1a05-419a-a503-60013f111c8a
ms.openlocfilehash: 9b77c44aa77afd59e36e1d21451205d3929ef527
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90874873"
---
# <a name="amp-operator-visual-basic"></a>&amp;= 运算符 (Visual Basic)

将 `String` 表达式连接到 `String` 变量或属性，并将结果赋给变量或属性。  
  
## <a name="syntax"></a>语法  
  
```vb  
variableorproperty &= expression  
```  
  
## <a name="parts"></a>组成部分  

 `variableorproperty`  
 必需。 任何 `String` 变量或属性。  
  
 `expression`  
 必需。 任何 `String` 表达式。  
  
## <a name="remarks"></a>备注  

 运算符左侧的元素 `&=` 可以是简单的标量变量、属性或数组的元素。 变量或属性不能是 [只读](../modifiers/readonly.md)的。 `&=`运算符将 `String` 其右侧的表达式连接到左侧的 `String` 变量或属性，并将结果赋给其左侧的变量或属性。  
  
## <a name="overloading"></a>重载  

 可以*重载* [& 运算符](concatenation-operator.md)，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 重载 `&` 运算符会影响运算符的行为 `&=` 。 如果你的代码 `&=` 在重载的类或结构上使用 `&` ，请确保你了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `&=` 运算符将两个变量连接起来 `String` ，并将结果赋给第一个变量。  
  
 [!code-vb[VbVbalrOperators#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>另请参阅

- [& 运算符](concatenation-operator.md)
- [+ = 运算符](addition-assignment-operator.md)
- [赋值运算符](assignment-operators.md)
- [串联运算符](concatenation-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [语句](../../programming-guide/language-features/statements.md)
