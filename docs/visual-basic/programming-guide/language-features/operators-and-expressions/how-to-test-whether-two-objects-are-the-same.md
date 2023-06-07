---
title: 如何：测试两个对象是否相同
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], reference
- Is operator [Visual Basic], comparing objects
- reference variables [Visual Basic]
- variables [Visual Basic], referring to same object
- objects [Visual Basic], variables referring to same
- Visual Basic code, operators
ms.assetid: f760e828-8704-4256-bc2d-c22a4c93b524
ms.openlocfilehash: d29d1b0026b3f62d47859cd5b4b7a601532e27b8
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071685"
---
# <a name="how-to-test-whether-two-objects-are-the-same-visual-basic"></a>如何：测试两个对象是否相同 (Visual Basic)

如果有两个引用对象的变量，则可以使用 `Is` 或 `IsNot` 运算符，或同时使用这两个变量来确定它们是否引用相同的实例。  
  
### <a name="to-test-whether-two-objects-are-the-same"></a>测试两个对象是否相同  
  
- 使用 [Is 运算符](../../../language-reference/operators/is-operator.md) 或将两个变量作为操作数的 [IsNot 运算符](../../../language-reference/operators/isnot-operator.md) 。  
  
     [!code-vb[VbVbalrOperators#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#69)]  
  
 您可能想要执行特定的操作，具体取决于两个对象是否引用相同的实例。 前面的示例将控制 `c` 与窗体上的活动控件进行比较 `f` 。 如果没有活动的控件，或者存在，但它不是与相同的控件实例 `c` ，则 `If` 语句将失败，并且过程将返回，而不进行进一步的处理。  
  
 你使用 `Is` 或是否 `IsNot` 是你的个人便利。 比给定表达式中的另一个更易于读取。  
  
## <a name="see-also"></a>请参阅

- [Comparison Operators in Visual Basic](comparison-operators.md)
