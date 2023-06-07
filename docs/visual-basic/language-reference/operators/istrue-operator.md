---
title: IsTrue 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.istrue
helpviewer_keywords:
- IsTrue operator [Visual Basic]
- OrElse operator [Visual Basic]
ms.assetid: b6cec0f2-61b1-4331-a7f0-4d07ee3179d6
ms.openlocfilehash: 2e67a4adabe58ab12d317ae6318c0a2fac29da7d
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866944"
---
# <a name="istrue-operator-visual-basic"></a>IsTrue 运算符 (Visual Basic)

确定表达式是否为 `True` 。  
  
 你不能 `IsTrue` 在代码中显式调用，但是 Visual Basic 编译器可以使用它来生成代码 from `OrElse` 子句。 如果定义类或结构，然后在子句中使用该类型的变量 `OrElse` ，则必须 `IsTrue` 在该类或结构上进行定义。  
  
 编译器将 `IsTrue` 和运算符视为 `IsFalse` *匹配的对*。 这意味着，如果定义其中一个类型，则还必须定义另一个。  
  
## <a name="compiler-use-of-istrue"></a>IsTrue 的编译器使用  

 定义了类或结构后，可以在 `For` 、 `If` 、 `Else If` 或 `While` 语句或 `When` 子句中使用该类型的变量。 如果执行此操作，则编译器需要一个运算符，该运算符将您的类型转换为 `Boolean` 值，以便可以测试条件。 它按以下顺序搜索合适的运算符：  
  
1. 从你的类或结构到的扩大转换运算符 `Boolean` 。  
  
2. 从你的类或结构到的扩大转换运算符 `Boolean?` 。  
  
3. `IsTrue`类或结构中的运算符。  
  
4. 到的收缩转换 `Boolean?` 不涉及从到的转换 `Boolean` `Boolean?` 。  
  
5. 类或结构中的收缩转换运算符 `Boolean` 。  
  
 如果未定义任何到 `Boolean` 或运算符的转换 `IsTrue` ，则编译器会发出错误消息。  
  
> [!NOTE]
> `IsTrue`运算符可以*重载*，这意味着当类或结构的操作数具有该类或结构的类型时，该类或结构可以重新定义它的行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例定义了包含和运算符定义的结构的轮廓 `IsFalse` `IsTrue` 。  
  
 [!code-vb[VbVbalrOperators#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>另请参阅

- [IsFalse 运算符](isfalse-operator.md)
- [如何：定义运算符](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [OrElse 运算符](orelse-operator.md)
