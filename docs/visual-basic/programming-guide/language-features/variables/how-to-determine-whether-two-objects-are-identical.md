---
title: 如何：确定两个对象是否相同
ms.date: 07/20/2015
helpviewer_keywords:
- testing [Visual Basic], objects
- objects [Visual Basic], comparing
- object variables [Visual Basic], determining identity
ms.assetid: 7829f817-0d1f-4749-a707-de0b95e0cf5c
ms.openlocfilehash: 1bbc8083fcfb6f5ff0f4328c32b83a2e7218ecd6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072270"
---
# <a name="how-to-determine-whether-two-objects-are-identical-visual-basic"></a>如何：确定两个对象是否相同 (Visual Basic)

在 Visual Basic 中，如果两个变量引用相同，即两个变量都指向内存中的同一个类实例，则将其视为相同。 例如，在 Windows 窗体应用程序中，您可能需要进行比较以确定当前的实例是否 () 与 `Me` 特定实例相同，例如 `Form2` 。  
  
 Visual Basic 提供了两个运算符来比较指针。 如果对象相同， [则 Is 运算符](../../../language-reference/operators/is-operator.md) 返回 `True` ; 如果不是，则为 [IsNot 运算符](../../../language-reference/operators/isnot-operator.md) `True` 。  
  
## <a name="determining-if-two-objects-are-identical"></a>确定两个对象是否相同  
  
#### <a name="to-determine-if-two-objects-are-identical"></a>确定两个对象是否相同  
  
1. 设置 `Boolean` 用于测试两个对象的表达式。  
  
2. 在测试表达式中，对 `Is` 两个对象使用运算符作为操作数。  
  
     `Is``True`如果对象指向相同的类实例，则返回。  
  
## <a name="determining-if-two-objects-are-not-identical"></a>确定两个对象是否不相同  

 有时，如果两个对象不完全相同，则需要执行一个操作，例如，将和组合起来会很难 `Not` `Is` `If Not obj1 Is obj2` 。 在这种情况下，可以使用 `IsNot` 运算符。  
  
#### <a name="to-determine-if-two-objects-are-not-identical"></a>确定两个对象是否不相同  
  
1. 设置 `Boolean` 用于测试两个对象的表达式。  
  
2. 在测试表达式中，对 `IsNot` 两个对象使用运算符作为操作数。  
  
     `IsNot``True`如果对象未指向同一类实例，则返回。  
  
## <a name="example"></a>示例  

 下面的示例测试变量对 `Object` ，以确定它们是否指向相同的类实例。  
  
 [!code-vb[VbVbalrKeywords#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class7.vb#14)]  
  
 前面的示例显示以下输出。  
  
 `objA different from objB? True`  
  
 `objA identical to objC? True`  
  
## <a name="see-also"></a>请参阅

- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
- [对象变量](object-variables.md)
- [对象变量值](object-variable-values.md)
- [Is 运算符](../../../language-reference/operators/is-operator.md)
- [IsNot 运算符](../../../language-reference/operators/isnot-operator.md)
- [如何：确定两个对象是否相关](how-to-determine-whether-two-objects-are-related.md)
- [Me、My、MyBase 和 MyClass](../../program-structure/me-my-mybase-and-myclass.md)
