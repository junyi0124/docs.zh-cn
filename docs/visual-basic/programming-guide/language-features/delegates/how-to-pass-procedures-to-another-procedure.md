---
title: 如何：将过程传递给另一过程
ms.date: 07/20/2015
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- delegates [Visual Basic], passing procedures
ms.assetid: 5adbba15-5a1d-413f-ab3e-3ff6cc0a4669
ms.openlocfilehash: 3a7a653bbf238b50e3c7339da76df0f68ab9b59f
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085784"
---
# <a name="how-to-pass-procedures-to-another-procedure-in-visual-basic"></a>如何：在 Visual Basic 中将过程传递给另一过程

此示例演示如何使用委托将过程传递给另一个过程。  
  
 委托是一种类型，您可以像在 Visual Basic 中那样使用任何其他类型。 `AddressOf`运算符在应用于过程名称时返回一个委托对象。  
  
 此示例包含一个带有委托参数的过程，该参数可以引用另一个过程，并使用 `AddressOf` 运算符获得。  
  
### <a name="create-the-delegate-and-matching-procedures"></a>创建委托和匹配过程  
  
1. 创建一个名为的委托 `MathOperator` 。  
  
     [!code-vb[VbVbalrDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#1)]  
  
2. `AddNumbers`使用与相同的参数和返回值创建一个名为的过程 `MathOperator` ，使签名匹配。  
  
     [!code-vb[VbVbalrDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#2)]  
  
3. 使用匹配的签名创建一个名为的过程 `SubtractNumbers` `MathOperator` 。  
  
     [!code-vb[VbVbalrDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#3)]  
  
4. 创建一个名为 `DelegateTest` 的过程，该过程使用委托作为参数。  
  
     此过程可以接受对或的 `AddNumbers` 引用 `SubtractNumbers` ，因为其签名与 `MathOperator` 签名匹配。  
  
     [!code-vb[VbVbalrDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#4)]  
  
5. 创建一个名为 `Test` 的过程，该过程 `DelegateTest` 使用委托 `AddNumbers` 作为参数，并再次使用的委托 `SubtractNumbers` 作为参数。  
  
     [!code-vb[VbVbalrDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#5)]  
  
     `Test`调用时，它首先显示 `AddNumbers` 对和的操作结果， `5` 即 `3` 8。 然后 `SubtractNumbers` ，将显示对和的操作结果，即 `9` `3` 6。  
  
## <a name="see-also"></a>请参阅

- [委托](index.md)
- [AddressOf 运算符](../../../language-reference/operators/addressof-operator.md)
- [Delegate 语句](../../../language-reference/statements/delegate-statement.md)
- [如何：调用委托方法](how-to-invoke-a-delegate-method.md)
