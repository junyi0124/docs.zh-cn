---
title: 教程：使用特性 - C#
description: 了解特性在 C# 中的工作方式。
author: mgroves
ms.technology: csharp-fundamentals
ms.date: 03/06/2017
ms.assetid: b152cf36-76e4-43a5-b805-1a1952e53b79
ms.openlocfilehash: 4e2c0126d0920df18271f8889d8e117cd374d979
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91174181"
---
# <a name="use-attributes-in-c"></a>在 C\# 中使用属性

使用特性，可以声明的方式将信息与代码相关联。 特性还可以提供能够应用于各种目标的可重用元素。

以 `[Obsolete]` 特性为例。 它可以应用于类、结构、方法、构造函数等。 用于_声明_元素已过时。 然后，由 C# 编译器负责查找此特性，并执行某响应操作。

此教程将介绍如何将特性添加到代码中、如何创建和使用你自己的特性，以及如何使用一些内置到 .NET Core 中的特性。

## <a name="prerequisites"></a>先决条件

必须将计算机设置为运行 .Net Core。 有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。
可以在 Windows、Ubuntu Linux、macOS 或 Docker 容器中运行此应用程序。
必须安装常用的代码编辑器。 在以下说明中，我们使用的是开放源代码跨平台编辑器 [Visual Studio Code](https://code.visualstudio.com/)。 不过，你可以使用习惯使用的任意工具。

## <a name="create-the-application"></a>创建应用程序

至此，你已安装所有工具，是时候新建 .NET Core 应用程序了。 若要使用命令行生成器，请在常用的命令行管理程序中执行以下命令：

`dotnet new console`

此命令将创建基本的 .NET Core 项目文件。 需要执行 `dotnet restore` 来还原编译此项目所需的依赖项。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

若要运行程序，请使用 `dotnet run`。 此时，应该可以在控制台中看到“Hello, World”输出。

## <a name="how-to-add-attributes-to-code"></a>如何将特性添加到代码中

在 C# 中，特性是继承自 `Attribute` 基类的类。 所有继承自 `Attribute` 的类都可以用作其他代码块的一种“标记”。
例如，有一个名为 `ObsoleteAttribute` 的特性。 它用于示意代码已过时，不得再使用。 可以将此特性应用于类（比如说，使用方括号）。

[!code-csharp[Obsolete attribute example](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ObsoleteExample1)]

请注意，虽然此类的名称为 `ObsoleteAttribute`，但只需在代码中使用 `[Obsolete]`。 这是 C# 遵循一项约定。
如果愿意，也可以使用全名 `[ObsoleteAttribute]`。

如果将类标记为已过时，最好说明已过时的*原因*和/或改用的*类*。 为此，可将字符串参数传递给 Obsolete 特性。

[!code-csharp[Obsolete attribute example with parameters](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ObsoleteExample2)]

此字符串会作为自变量传递给 `ObsoleteAttribute` 构造函数，就像在编写 `var attr = new ObsoleteAttribute("some string")` 一样。

只能向特性构造函数传递以下简单类型/文本类型参数：`bool, int, double, string, Type, enums, etc`和这些类型的数组。
不能使用表达式或变量。 可以使用任何位置参数或已命名参数。

## <a name="how-to-create-your-own-attribute"></a>如何创建你自己的特性

创建特性与从 `Attribute` 基类继承一样简单。

[!code-csharp[Create your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CreateAttributeExample1)]

执行上述操作后，我现在可以在基本代码中的其他位置使用 `[MySpecial]`（或 `[MySpecialAttribute]`）特性。

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CreateAttributeExample2)]

.NET 基类库中的特性（如 `ObsoleteAttribute`）会在编译器中触发某些行为。 不过，你创建的任何特性只作元数据之用，并不会在执行的特性类中生成任何代码。 是否在代码的其他位置使用此元数据由你自行决定（此教程稍后将对此进行详细介绍）。

这里要注意的是“gotcha”。 如上所述，使用特性时，只允许将某些类型的参数作为自变量传递。 不过，在创建特性类型时，C# 编译器不会阻止你创建这些参数。 在以下示例中，我使用可正常编译的构造函数创建了特性。

[!code-csharp[Valid constructor used in an attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeGothca1)]

不过，无法将此构造函数与特性语法结合使用。

[!code-csharp[Invalid attempt to use the attribute constructor](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeGotcha2)]

上述做法会导致生成编译器错误，如 `Attribute constructor parameter 'myClass' has type 'Foo', which is not a valid attribute parameter type`

## <a name="how-to-restrict-attribute-usage"></a>如何限制特性使用

特性可用于多个“目标”。 上述示例展示了特性在类上的使用情况，而特性还可用于：

* 程序集
* 类
* 构造函数
* 委托
* 枚举
* 事件
* 字段
* 泛型参数
* 接口
* 方法
* 模块
* 参数
* properties
* 返回值
* 结构

创建特性类时，C# 默认允许对所有可能的特性目标使用此特性。 如果要将特性限制为只能用于特定目标，可以对特性类使用 `AttributeUsageAttribute` 来实现。 没错，就是将特性应用于特性！

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeUsageExample1)]

如果尝试将上述特性应用于不是类也不是结构的对象，则会看到编译器错误，如 `Attribute 'MyAttributeForClassAndStructOnly' is not valid on this declaration type. It is only valid on 'class, struct' declarations`

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeUsageExample2)]

## <a name="how-to-use-attributes-attached-to-a-code-element"></a>如何使用附加到代码元素的特性

特性只作元数据之用。 不借助一些外在力量，特性其实什么用也没有。

若要查找并使用特性，通常需要使用[反射](../programming-guide/concepts/reflection.md)。 我不会在此教程中深入介绍反射，但基本概念就是借助反射，可以在 C# 中编写用于检查其他代码的代码。

例如，可以使用反射获取类的相关信息（在代码开始处添加 `using System.Reflection;`）：

[!code-csharp[Getting type information with Reflection](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ReflectionExample1)]

此代码的打印输出如下：`The assembly qualified name of MyClass is ConsoleApplication.MyClass, attributes, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

获取 `TypeInfo` 对象（或 `MemberInfo`、`FieldInfo` 等）后，就可以使用 `GetCustomAttributes` 方法了。 这将返回一组 `Attribute` 对象。
还可以使用 `GetCustomAttribute` 并指定特性类型。

下面的示例展示了对 `MyClass`（在上文中，它包含 `[Obsolete]` 特性）的 `MemberInfo` 实例使用 `GetCustomAttributes`。

[!code-csharp[Getting type information with Reflection](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ReflectionExample2)]

这将在控制台中打印输出 `Attribute on MyClass: ObsoleteAttribute`。 请尝试向 `MyClass` 添加其他特性。

请务必注意，这些 `Attribute` 对象的实例化有延迟。 也就是说，只有使用 `GetCustomAttribute` 或 `GetCustomAttributes`，它们才会实例化。
这些对象每次都会实例化。 连续两次调用 `GetCustomAttributes` 将返回两个不同的 `ObsoleteAttribute` 实例。

## <a name="common-attributes-in-the-base-class-library-bcl"></a>基类库 (BCL) 中的常见特性

许多工具和框架都会使用特性。 NUnit 和 NUnit 测试运行程序都使用 `[Test]` 和 `[TestFixture]` 之类的特性。 ASP.NET MVC 使用 `[Authorize]` 之类的特性，并提供可密切关注 MVC 操作的操作筛选器框架。 [PostSharp](https://www.postsharp.net) 使用特性语法，支持在 C# 中进行面向特性的编程。

下面介绍了一些值得注意的 .NET Core 基类库内置特性：

* `[Obsolete]`. 此特性已在上面的示例中使用过，位于 `System` 命名空间中。 可用于提供关于不断变化的基本代码的声明性文档。 可以提供字符串形式的消息，并能使用另一布尔参数将编译器警告升级为编译器错误。

* `[Conditional]`. 此特性位于 `System.Diagnostics` 命名空间中。 可应用于方法（或特性类）。 必须向构造函数传递字符串。
如果该字符串与 `#define` 指令不匹配，那么 C# 编译器将删除对该方法（而不是方法本身）的所有调用。 此特性通常用于调试（诊断）目的。

* `[CallerMemberName]`. 此特性可应用于参数，位于 `System.Runtime.CompilerServices` 命名空间中。 可用于插入正在调用另一方法的方法的名称。 此特性通常用于在各种 UI 框架中实现 INotifyPropertyChanged 时清除“神奇字符串”。 示例：

[!code-csharp[Using CallerMemberName when implementing INotifyPropertyChanged](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CallerMemberName1)]

在上面的代码中，无需使用文本类型 `"Name"` 字符串。 这样既有助于防止出现与拼写错误相关的 bug，也可以让重构/重命名操作变得更加顺畅。

## <a name="summary"></a>总结

属性为 C# 带来了声明性能力，但它们是一种元数据形式的代码，本身并不执行操作。
