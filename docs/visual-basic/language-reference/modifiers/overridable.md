---
title: Overrides
ms.date: 07/20/2015
f1_keywords:
- Overridable
- vb.Overridable
helpviewer_keywords:
- elements [Visual Basic], concrete
- properties [Visual Basic], redefining
- overriding, Overridable keyword
- elements [Visual Basic], virtual
- virtual [elements VB]
- procedures [Visual Basic], overriding
- concrete [elements VB]
- procedures [Visual Basic], redefining
- Overridable keyword [Visual Basic]
- properties [Visual Basic], overriding
ms.assetid: 612581e7-8a4c-4a5d-beff-3402fffa6f35
ms.openlocfilehash: 8506aba7e64f2dbd975cc275cefb7b5bb1aefda5
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875009"
---
# <a name="overridable-visual-basic"></a>Overridable (Visual Basic)

指定属性或过程可由派生类中具有相同名称的属性或过程重写。  
  
## <a name="remarks"></a>备注  

 `Overridable`修饰符允许在派生类中重写类中的属性或方法。 [NotOverridable](notoverridable.md)修饰符防止在派生类中重写属性或方法。  有关详细信息，请参阅[继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)。  
  
 如果 `Overridable` `NotOverridable` 未指定或修饰符，则默认设置取决于属性或方法是重写基类属性还是替代方法。 如果属性或方法重写基类属性或方法，则默认设置为 `Overridable` ; 否则为 `NotOverridable` 。  
  
 您可以隐藏或重写来重新定义继承的元素，但这两种方法之间的差异很大。 有关详细信息，请参阅 [Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)。  
  
 可以重写的元素有时称为 *虚拟* 元素。 如果可以重写，但不一定是，则有时也称为 *具体* 元素。  
  
 `Overridable`只能在属性或过程声明语句中使用。  
  
## <a name="combined-modifiers"></a>组合修饰符  

 不能 `Overridable` `NotOverridable` 为方法指定或 `Private` 。  
  
 不能 `Overridable` `MustOverride` `NotOverridable` `Shared` 在同一声明中同时指定、或。  
  
 由于重写元素是隐式可重写的，因此不能将 `Overridable` 与 `Overrides` 组合到一起。  
  
## <a name="usage"></a>使用情况  

 `Overridable` 修饰符可用于下面的上下文中：  
  
 [Function 语句](../statements/function-statement.md)  
  
 [Property Statement](../statements/property-statement.md)  
  
 [Sub 语句](../statements/sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [修饰符](index.md)
- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [New](mustoverride.md)
- [NotOverridable](notoverridable.md)
- [替代](overrides.md)
- [关键字](../keywords/index.md)
- [Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)
