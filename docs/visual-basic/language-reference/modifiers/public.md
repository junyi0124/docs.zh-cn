---
title: 公用
ms.date: 07/20/2015
f1_keywords:
- vb.Public
helpviewer_keywords:
- Public keyword [Visual Basic]
- Public keyword [Visual Basic], syntax
- Public access modifier
ms.assetid: 284c9e1b-ed23-499b-9bc9-ad87c11485a5
ms.openlocfilehash: f2b6a126435b111ef56ee2a9870ea6fbddf87901
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867683"
---
# <a name="public-visual-basic"></a>Public (Visual Basic)

指定一个或多个已声明的编程元素不具有访问限制。  
  
## <a name="remarks"></a>备注  

 如果要发布一个组件或一组组件（如类库），通常会希望与程序集互操作的任何代码都可以访问编程元素。 若要在某个元素上协商这种无限制的访问权限，可以使用对其进行声明 `Public` 。  
  
 当不需要限制对编程元素的访问权限时，公共访问是该元素的正常级别。 请注意，在接口、模块、类或结构中声明的元素的访问级别默认为（ `Public` 如果你未声明）。  
  
## <a name="rules"></a>规则  
  
- **声明上下文。** `Public`只能在模块、接口或命名空间级别使用。 这意味着元素的声明上下文 `Public` 必须是源文件、命名空间、接口、模块、类或结构，不能是过程。  
  
## <a name="behavior"></a>行为  
  
- **访问级别。** 可以访问模块、类或结构的所有代码都可以访问其 `Public` 元素。  
  
- **默认访问权限。** 过程中的局部变量默认为公共访问，您不能对其使用任何访问修饰符。  
  
- **访问修饰符。** 指定访问级别的关键字称为 *访问修饰符*。 有关访问修饰符的比较，请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。  
  
 `Public` 修饰符可用于下面的上下文中：  
  
 [Class 语句](../statements/class-statement.md)  
  
 [Const 语句](../statements/const-statement.md)  
  
 [Declare Statement](../statements/declare-statement.md)  
  
 [Delegate 语句](../statements/delegate-statement.md)  
  
 [Dim 语句](../statements/dim-statement.md)  
  
 [Enum 语句](../statements/enum-statement.md)  
  
 [Event 语句](../statements/event-statement.md)  
  
 [Function 语句](../statements/function-statement.md)  
  
 [Interface 语句](../statements/interface-statement.md)  
  
 [Module 语句](../statements/module-statement.md)  
  
 [Operator Statement](../statements/operator-statement.md)  
  
 [Property Statement](../statements/property-statement.md)  
  
 [Structure 语句](../statements/structure-statement.md)  
  
 [Sub 语句](../statements/sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [避免](protected.md)
- [Friend](friend.md)
- [专用](private.md)
- [私有受保护](private-protected.md)
- [Protected Friend](protected-friend.md)
- [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)
- [过程](../../programming-guide/language-features/procedures/index.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
