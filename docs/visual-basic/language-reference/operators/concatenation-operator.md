---
title: '&amp; 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.&
helpviewer_keywords:
- And (&) operator
- ampersand operator (&)
- '& operator'
- concatenation operators [Visual Basic], syntax
- strings [Visual Basic], concatenating
ms.assetid: fefc3d00-cbf1-475c-8c5e-6fb213b3f85a
ms.openlocfilehash: 20c2e2088691e68221872cc1dfc5486413515a4d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867148"
---
# <a name="amp-operator-visual-basic"></a>&amp; 操作员 (Visual Basic) 

生成两个表达式的字符串串联。  
  
## <a name="syntax"></a>语法  
  
```vb  
result = expression1 & expression2  
```  
  
## <a name="parts"></a>组成部分  

 `result`  
 必需。 Any `String` 或 `Object` variable。  
  
 `expression1`  
 必需。 数据类型扩大到的任何表达式 `String` 。  
  
 `expression2`  
 必需。 数据类型扩大到的任何表达式 `String` 。  
  
## <a name="remarks"></a>备注  

 如果或的数据类型 `expression1` `expression2` 不能 `String` 扩大到，则 `String` 会将其转换为 `String` 。 如果数据类型之一不能扩大到 `String` ，编译器将生成错误。  
  
 的数据类型 `result` 为 `String` 。 如果一个或两个表达式的计算结果都不为，或者其 [值为](../nothing.md) <xref:System.DBNull.Value?displayProperty=nameWithType> ，则将它们视为值为 "" 的字符串。  
  
> [!NOTE]
> `&`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
> [!NOTE]
> 与号 ( # A0) 字符也可用于将变量标识为类型 `Long` 。 有关详细信息，请参阅 [类型字符](../../programming-guide/language-features/data-types/type-characters.md)。  
  
## <a name="example"></a>示例  

 此示例使用 `&` 运算符强制字符串连接。 结果是一个表示两个字符串操作数串联的字符串值。  
  
 [!code-vb[VbVbalrOperators#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [&= 运算符](and-assignment-operator.md)
- [串联运算符](concatenation-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [串联运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/concatenation-operators.md)
