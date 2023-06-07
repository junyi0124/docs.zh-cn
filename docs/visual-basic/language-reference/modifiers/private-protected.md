---
title: Private Protected
ms.date: 05/10/2018
f1_keywords:
- vb.PrivateProtected
helpviewer_keywords:
- Private Protected keyword [Visual Basic]
- Private Protected keyword [Visual Basic], syntax
ms.openlocfilehash: 8ad1509da71bc80b33700d363ddd4569a0965dff
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303460"
---
# <a name="private-protected-visual-basic"></a>私有受保护（Visual Basic）

`Private Protected` 关键字组合是一种成员访问修饰符。 `Private Protected`成员可以由其包含类中的所有成员访问，也可由派生自包含类的类型访问，但前提是在它的包含程序集中找到。

只能 `Private Protected` 在类的成员上指定; 无法应用 `Private Protected` 于结构的成员，因为结构不能继承。

`Private Protected`Visual Basic 15.5 及更高版本支持访问修饰符。 若要使用它，可以将以下元素添加到 Visual Basic 项目（ \* .vbproj）文件中。 只要您的系统上安装了 Visual Basic 15.5 或更高版本，就可以利用 Visual Basic 编译器最新版本支持的所有语言功能：

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

有关详细信息，请参阅[设置 Visual Basic 语言版本](../configure-language-version.md)。

> [!NOTE]
> 在 Visual Studio 中，选择上的 F1 帮助 `private protected` 可为[私有](private.md)或[受保护](protected.md)提供帮助。 IDE 将选取光标下的单个标记，而不是组合词。

## <a name="rules"></a>规则

- **声明上下文。** `Private Protected`只能在类级别使用。 这意味着元素的声明上下文 `Protected` 必须是类，且不能是源文件、命名空间、接口、模块、结构或过程。

## <a name="behavior"></a>行为

- **访问级别。** 类中的所有代码都可以访问其元素。 派生自基类并包含在同一程序集中的任何类中的代码都可以访问基类的所有 `Private Protected` 元素。 但是，任何派生自基类的类中的代码都包含在不同的程序集中，因此无法访问基类 `Private Protected` 元素。

- **访问修饰符。** 指定访问级别的关键字称为*访问修饰符*。 有关访问修饰符的比较，请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

`Private Protected` 修饰符可用于下面的上下文中：

- 嵌套类的[Class 语句](../statements/class-statement.md)

- [Const 语句](../statements/const-statement.md)

- [Declare Statement](../statements/declare-statement.md)

- 嵌套在类中的委托的[委托语句](../statements/delegate-statement.md)

- [Dim 语句](../statements/dim-statement.md)

- 嵌套在类中的枚举的[枚举语句](../statements/enum-statement.md)

- [Event 语句](../statements/event-statement.md)

- [Function 语句](../statements/function-statement.md)

- 嵌套在类中的接口的[接口语句](../statements/interface-statement.md)

- [Property Statement](../statements/property-statement.md)

- 嵌套在类中的结构的[结构语句](../statements/structure-statement.md)

- [Sub 语句](../statements/sub-statement.md)

## <a name="see-also"></a>另请参阅

- [公共](public.md)
- [避免](protected.md)
- [友好](friend.md)
- 专用
- [Protected Friend](./protected-friend.md)
- [Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)
- [过程](../../programming-guide/language-features/procedures/index.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
