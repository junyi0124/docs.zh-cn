---
title: NotOverridable
ms.date: 07/20/2015
f1_keywords:
- vb.NotOverridable
- NotOverridable
helpviewer_keywords:
- sealed methods [Visual Basic]
- NotOverridable keyword [Visual Basic]
- properties [Visual Basic], redefining
- elements [Visual Basic], sealed
- sealed [elements VB]
- procedures [Visual Basic], overriding
- procedures [Visual Basic], redefining
- overriding
- methods [Visual Basic], sealed
- properties [Visual Basic], overriding
ms.assetid: 66ec6984-f5f5-4857-b362-6a3907aaf9e0
ms.openlocfilehash: 8eec54a12c7fb748df46e8c48a8b07eab983cc72
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867871"
---
# <a name="notoverridable-visual-basic"></a>NotOverridable (Visual Basic)

指定不能在派生类中重写属性或过程。  
  
## <a name="remarks"></a>备注  

 `NotOverridable`修饰符可防止在派生类中重写属性或方法。  可 [重写](overridable.md) 的修饰符允许在派生类中重写类中的属性或方法。 有关详细信息，请参阅[继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)。  
  
 如果 `Overridable` `NotOverridable` 未指定或修饰符，则默认设置取决于属性或方法是重写基类属性还是替代方法。 如果属性或方法重写基类属性或方法，则默认设置为 `Overridable` ; 否则为 `NotOverridable` 。  
  
 不能重写的元素有时称为 *密封* 元素。  
  
 `NotOverridable`只能在属性或过程声明语句中使用。 只能 `NotOverridable` 在替代另一个属性或过程的属性或过程上指定，即，只能与一起使用 `Overrides` 。  
  
## <a name="combined-modifiers"></a>组合修饰符  

 不能 `Overridable` `NotOverridable` 为方法指定或 `Private` 。  
  
 不能 `NotOverridable` `MustOverride` `Overridable` `Shared` 在同一声明中同时指定、或。  
  
## <a name="usage"></a>使用情况  

 `NotOverridable` 修饰符可用于下面的上下文中：  
  
 [Function 语句](../statements/function-statement.md)  
  
 [Property Statement](../statements/property-statement.md)  
  
 [Sub 语句](../statements/sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [修饰符](index.md)
- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [New](mustoverride.md)
- [Overrides](overridable.md)
- [替代](overrides.md)
- [关键字](../keywords/index.md)
- [Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)
