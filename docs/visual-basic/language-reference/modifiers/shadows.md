---
title: Shadows
ms.date: 07/20/2015
f1_keywords:
- vb.Shadows
- shadows
helpviewer_keywords:
- shadowing
- duplicate names [Visual Basic]
- scope [Visual Basic], shadowing
- Shadows keyword [Visual Basic]
- names [Visual Basic], shadowing
ms.assetid: 6bf687cd-0544-4797-b51b-911125ec57c6
ms.openlocfilehash: 7aed6bec21bd484cca019b061bd5915de13a9eb8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402702"
---
# <a name="shadows-visual-basic"></a>Shadows (Visual Basic)

指定已声明的编程元素重新声明并隐藏基类中具有相同名称的元素或重载元素集。

## <a name="remarks"></a>备注

隐藏（也称为*按名称隐藏*）的主要目的是保留类成员的定义。 基类可能会发生更改，该更改将创建一个与已定义的元素同名的元素。 如果发生这种情况， `Shadows` 修饰符会强制通过类的引用解析为你定义的成员而不是新的基类元素。

隐藏和重写操作都可重新定义继承的元素，但这两种方法之间又具有很大的差异。 有关详细信息，请参阅[Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)。

## <a name="rules"></a>规则

- **声明上下文。** 只能 `Shadows` 在类级别使用。 这意味着元素的声明上下文 `Shadows` 必须是类，且不能是源文件、命名空间、接口、模块、结构或过程。

  只能在单个声明语句中声明一个隐藏元素。

- **组合修饰符。** 不能 `Shadows` `Overloads` `Overrides` `Static` 在同一声明中同时指定、或。

- **元素类型。** 可以与任何其他类型一起隐藏任何类型的已声明元素。 如果使用另一个属性或过程来隐藏属性或过程，则参数和返回类型不必与基类属性或过程中的相同。

- **访问.** 通常，基类中隐藏的元素在隐藏它的派生类中不可用。 不过，请注意以下事项。

  - 如果不能从引用它的代码访问隐藏元素，则会将引用解析为隐藏的元素。 例如，如果某个 `Private` 元素隐藏了一个基类元素，则没有访问该元素的权限的代码将 `Private` 改为访问该基类元素。

  - 如果隐藏元素，仍然可以通过使用基类的类型声明的对象访问隐藏的元素。 你还可以通过来访问它 `MyBase` 。

`Shadows` 修饰符可用于下面的上下文中：

- [Class 语句](../statements/class-statement.md)

- [Const 语句](../statements/const-statement.md)

- [Declare Statement](../statements/declare-statement.md)

- [Delegate 语句](../statements/delegate-statement.md)

- [Dim 语句](../statements/dim-statement.md)

- [Enum 语句](../statements/enum-statement.md)

- [Event 语句](../statements/event-statement.md)

- [Function 语句](../statements/function-statement.md)

- [Interface 语句](../statements/interface-statement.md)

- [Property Statement](../statements/property-statement.md)

- [Structure 语句](../statements/structure-statement.md)

- [Sub 语句](../statements/sub-statement.md)

## <a name="see-also"></a>另请参阅

- [共享](shared.md)
- [静态](static.md)
- 专用 
- [Me、My、MyBase 和 MyClass](../../programming-guide/program-structure/me-my-mybase-and-myclass.md)
- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [New](mustoverride.md)
- [NotOverridable](notoverridable.md)
- [重载](overloads.md)
- [Overrides](overridable.md)
- [替代](overrides.md)
- [Visual Basic 中的隐藏](../../programming-guide/language-features/declared-elements/shadowing.md)
