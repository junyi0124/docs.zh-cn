---
title: Friend
ms.date: 07/20/2015
f1_keywords:
- vb.Friend
helpviewer_keywords:
- Friend keyword [Visual Basic]
- Friend access modifier
- Friend keyword [Visual Basic], syntax
- Protected Friend keyword combination
- Friend keyword [Visual Basic], and Protected
ms.assetid: b664605e-1c79-4728-b996-aa59c50846bc
ms.openlocfilehash: d37a93343822d069295477958780c2b9c72043fa
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875458"
---
# <a name="friend-visual-basic"></a>Friend (Visual Basic)

指定只能从包含其声明的程序集内部访问一个或多个已声明的编程元素。  
  
## <a name="remarks"></a>备注  

 在许多情况下，需要由整个程序集使用的编程元素（如类和结构），而不是由声明它们的组件使用。 不过，如果应用程序是专有) ，则可能不希望程序集外部的代码可以访问它们 (例如，。 如果要以这种方式限制对某个元素的访问，则可以使用修饰符来声明它 `Friend` 。  
  
 编译为同一程序集的其他类、结构和模块中的代码可以访问 `Friend` 该程序集中的所有元素。  
  
 `Friend` 访问通常是应用程序编程元素的首选级别， `Friend` 是接口、模块、类或结构的默认访问级别。  
  
 `Friend`只能在模块、接口或命名空间级别使用。 因此，元素的声明上下文 `Friend` 必须是源文件、命名空间、接口、模块、类或结构; 它不能是过程。  

> [!NOTE]
> 你还可以使用 [受保护的 Friend](protected-friend.md) 访问修饰符，使类成员可从该类、派生类和类定义的同一程序集中访问。 若要从同一程序集中的类和派生类中限制对成员的访问，请使用 [私有受保护](private-protected.md) 的访问修饰符。

 有关 `Friend` 和其他访问修饰符的比较，请参阅 [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。  
  
> [!NOTE]
> 可以指定另一个程序集是友元程序集，该程序集允许其访问标记为的所有类型和成员 `Friend` 。 有关详细信息，请参阅[友元程序集](../../../standard/assembly/friend.md)。

## <a name="example"></a>示例  

 下面的类使用 `Friend` 修饰符允许同一程序集中的其他编程元素访问某些成员。  
  
 [!code-vb[VbVbalrAccessModifiers#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalraccessmodifiers/vb/class1.vb#1)]  
  
## <a name="usage"></a>使用情况  

 可以 `Friend` 在以下上下文中使用修饰符：  
  
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
  
 [Property Statement](../statements/property-statement.md)  
  
 [Structure 语句](../statements/structure-statement.md)  
  
 [Sub 语句](../statements/sub-statement.md)  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- [公共](public.md)
- [避免](protected.md)
- [专用](private.md)
- [私有受保护](./private-protected.md)
- [Protected Friend](./protected-friend.md)
- [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)
- [过程](../../programming-guide/language-features/procedures/index.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
