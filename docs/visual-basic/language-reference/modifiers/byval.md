---
title: ByVal
ms.date: 07/20/2015
f1_keywords:
- vb.ByVal
- ByVal
helpviewer_keywords:
- ByVal keyword [Visual Basic], contexts
- ByVal keyword [Visual Basic]
ms.assetid: 1eaf4e58-b305-4785-9e3d-e416b9c75598
ms.openlocfilehash: 8daf6600f59cea343e0d02b99632b92ce4bf3183
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875478"
---
# <a name="byval-visual-basic"></a>ByVal (Visual Basic)

指定参数 [按值](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)传递，因此被调用的过程或属性无法更改调用代码中参数的基础变量的值。 如果未指定修饰符，则 ByVal 为默认值。

> [!NOTE]
> 由于它是默认值，因此不必 `ByVal` 在方法签名中显式指定关键字。 它往往会产生干扰代码，并经常导致非默认 `ByRef` 关键字被忽略。

## <a name="remarks"></a>备注

 `ByVal` 修饰符可用于下面的上下文中：

 [Declare Statement](../statements/declare-statement.md)

 [Function 语句](../statements/function-statement.md)
  
 [Operator Statement](../statements/operator-statement.md)
  
 [Property Statement](../statements/property-statement.md)
  
 [Sub 语句](../statements/sub-statement.md)

## <a name="example"></a>示例

 下面的示例演示如何将 `ByVal` 参数传递机制与引用类型参数一起使用。 在此示例中，自变量是 `c1` 类的实例 `Class1` 。 `ByVal` 阻止过程中的代码更改 reference 参数的基础值，但不保护的 `c1` 可访问字段和属性 `c1` 。

 [!code-vb[VbVbalrKeywords#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class5.vb#10)]

## <a name="see-also"></a>另请参阅

- [关键字](../keywords/index.md)
- [按值和按引用传递参数](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
