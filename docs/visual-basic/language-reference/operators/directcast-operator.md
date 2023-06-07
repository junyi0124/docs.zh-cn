---
title: DirectCast 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.directCast
- directCast
helpviewer_keywords:
- DirectCast keyword [Visual Basic]
ms.assetid: 63e5a1d0-4d9e-4732-bf8f-e90c0c8784b8
ms.openlocfilehash: 7b070b8eba240440821f7984a9336c2ecaf61706
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867089"
---
# <a name="directcast-operator-visual-basic"></a>DirectCast 运算符 (Visual Basic)

引入基于继承或实现的类型转换操作。  
  
## <a name="remarks"></a>备注  

 `DirectCast` 不使用 Visual Basic 运行时帮助器例程进行转换，因此，它可以提供比 `CType` 转换到数据类型和从数据类型转换时更好的性能 `Object` 。  
  
 关键字的使用 `DirectCast` 方式类似于使用 [CType 函数](../functions/ctype-function.md) 和 [TryCast 运算符](trycast-operator.md) 关键字。 提供表达式作为第一个参数，并提供一个类型以将其转换为第二个参数。 `DirectCast` 需要两个自变量的数据类型之间的继承关系或实现关系。 这意味着一个类型必须从继承或实现另一个类型。  
  
## <a name="errors-and-failures"></a>错误和失败  

 `DirectCast` 如果检测到不存在继承或实现关系，则会生成编译器错误。 但缺少编译器错误并不能保证成功转换。 如果所需的转换是收缩转换，则可能会在运行时失败。 如果发生这种情况，运行时会引发 <xref:System.InvalidCastException> 错误。  
  
## <a name="conversion-keywords"></a>转换关键字  

 下面是类型转换关键字的比较。  
  
|关键字|数据类型|参数关系|运行时失败|  
|---|---|---|---|  
|[CType Function](../functions/ctype-function.md)|任何数据类型|必须在这两种数据类型之间定义扩大转换或收缩转换|却 <xref:System.InvalidCastException>|  
|`DirectCast`|任何数据类型|一个类型必须继承自或实现另一个类型|却 <xref:System.InvalidCastException>|  
|[TryCast 运算符](trycast-operator.md)|仅引用类型|一个类型必须继承自或实现另一个类型|不返回 [任何内容](../nothing.md)|  
  
## <a name="example"></a>示例  

 下面的示例演示了的两个用法 `DirectCast` ，一个在运行时失败，另一个成功。  
  
 [!code-vb[VbVbalrKeywords#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#1)]  
  
 在前面的示例中，的运行时类型 `q` 为 `Double` 。 `CType` 成功 `Double` ，因为可以转换为 `Integer` 。 但是，第一次 `DirectCast` 在运行时失败，因为的运行时类型 `Double` 与没有继承关系 `Integer` ，即使存在转换也是如此。 第二个 `DirectCast` 成功，因为它从类型转换 <xref:System.Windows.Forms.Form> 为类型 <xref:System.Windows.Forms.Control> ，后者 <xref:System.Windows.Forms.Form> 继承自。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Convert.ChangeType%2A?displayProperty=nameWithType>
- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
