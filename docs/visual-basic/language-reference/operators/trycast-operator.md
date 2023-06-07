---
title: TryCast 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.trycast
- trycast
helpviewer_keywords:
- TryCast keyword [Visual Basic]
ms.assetid: d1ef5d47-fef4-491e-b014-1d910628f65c
ms.openlocfilehash: dc4807781f9e1aaf894016952911bd7b32c42948
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875319"
---
# <a name="trycast-operator-visual-basic"></a>TryCast 运算符 (Visual Basic)

引入不引发异常的类型转换操作。  
  
## <a name="remarks"></a>备注  

 如果尝试的转换失败，则会 `CType` `DirectCast` 引发 <xref:System.InvalidCastException> 错误。 这会对应用程序的性能产生负面影响。 `TryCast` 不返回 [任何内容](../nothing.md)，因此只需测试返回的结果，而无需处理可能出现的异常 `Nothing` 。  
  
 `TryCast`关键字的使用方式与使用[CType 函数](../functions/ctype-function.md)和[DirectCast 运算符](directcast-operator.md)关键字相同。 提供表达式作为第一个参数，并提供一个类型以将其转换为第二个参数。 `TryCast` 仅对引用类型（如类和接口）进行操作。 它需要这两个类型之间的继承关系或实现关系。 这意味着一个类型必须从继承或实现另一个类型。  
  
## <a name="errors-and-failures"></a>错误和失败  

 `TryCast` 如果检测到不存在继承或实现关系，则会生成编译器错误。 但缺少编译器错误并不能保证成功转换。 如果所需的转换是收缩转换，则可能会在运行时失败。 如果发生这种情况，则不 `TryCast` 返回 [任何内容](../nothing.md)。  
  
## <a name="conversion-keywords"></a>转换关键字  

 下面是类型转换关键字的比较。  
  
|关键字|数据类型|参数关系|运行时失败|  
|---|---|---|---|  
|[CType Function](../functions/ctype-function.md)|任何数据类型|必须在这两种数据类型之间定义扩大转换或收缩转换|却 <xref:System.InvalidCastException>|  
|[DirectCast 运算符](directcast-operator.md)|任何数据类型|一个类型必须继承自或实现另一个类型|却 <xref:System.InvalidCastException>|  
|`TryCast`|仅引用类型|一个类型必须继承自或实现另一个类型|不返回 [任何内容](../nothing.md)|  
  
## <a name="example"></a>示例  

 下面的示例说明如何使用 `TryCast`。  
  
 [!code-vb[VbVbalrKeywords#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>另请参阅

- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
