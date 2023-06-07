---
title: Widening
ms.date: 07/20/2015
f1_keywords:
- vb.widening
helpviewer_keywords:
- conversions [Visual Basic], type
- type conversion [Visual Basic]
- conversions [Visual Basic], data type
- Widening keyword [Visual Basic]
- data type conversion [Visual Basic]
ms.assetid: 646ae263-94d3-40a2-b0cc-64f619292f56
ms.openlocfilehash: 14e0b026f4fc3b0bf202ea643a28d6f1a7df2b7c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867651"
---
# <a name="widening-visual-basic"></a>Widening (Visual Basic)

指示转换运算符 (`CType`) 将类或结构转换为可以保存原始类或结构的所有可能值的类型。  
  
## <a name="converting-with-the-widening-keyword"></a>用扩大关键字转换  

 除了之外，转换过程必须指定 `Public Shared` `Widening` 。  
  
 扩大转换始终会在运行时成功，不会导致数据丢失。 示例包括 `Single` 对 `Double` `Char` `String` 其基类型的、向和派生的类型。 由于派生类型包含基类型的所有成员，因此是最新的转换，因此是基类型的实例。  
  
 使用代码不必使用 `CType` 进行扩大转换，即使 `Option Strict` 是 `On` 。  
  
 `Widening`关键字可以在此上下文中使用：  
  
 [Operator Statement](../statements/operator-statement.md)  
  
 有关扩展和收缩转换运算符的定义示例，请参阅 [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)。  
  
## <a name="see-also"></a>另请参阅

- [Operator Statement](../statements/operator-statement.md)
- [Narrowing](narrowing.md)
- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [如何：定义运算符](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [CType Function](../functions/ctype-function.md)
- [Option Strict 语句](../statements/option-strict-statement.md)
- [如何：定义转换运算符](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
