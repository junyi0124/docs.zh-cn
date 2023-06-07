---
title: 'Visual Studio for Mac 中的 F # 入门'
description: '了解如何在 Visual Studio for Mac 中使用 F #。'
ms.date: 07/03/2018
ms.openlocfilehash: d2ad24ad18bb31419b39508f2cf555c82fbe2e0b
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437992"
---
# <a name="get-started-with-f-in-visual-studio-for-mac"></a>Visual Studio for Mac 中的 F # 入门

Visual Studio for Mac IDE 支持 F # 和 Visual F# 工具。 确保已 [安装 Visual Studio for Mac](install-fsharp.md#install-f-with-visual-studio-for-mac)。

## <a name="creating-a-console-application"></a>创建控制台应用程序

Visual Studio for Mac 中最基本的项目之一是控制台应用程序。  下面介绍如何执行该操作。  打开 Visual Studio for Mac 后：

1. 在 " **文件** " 菜单上，指向 " **新建解决方案**"。

2. 在 "新建项目" 对话框中，控制台应用程序有两个不同的模板。  在其他 > 的 .NET 中有一个面向 .NET Framework 的。  另一个模板位于 .NET Core 下，即面向 .NET Core > 应用。  这两个模板都适用于本文的目的。

3. 如果需要，在 "控制台应用" 下，将 c # 改为 F #。  选择 " **下一步** " 按钮继续！  

4. 为项目命名，并为应用选择所需的选项。  请注意，屏幕右侧的预览窗格将显示将根据所选选项创建的目录结构。  

5. 单击“创建”。  现在应会在解决方案资源管理器中看到一个 F # 项目。

## <a name="writing-your-code"></a>编写代码

让我们首先编写一些代码。  确保该 `Program.fs` 文件已打开，然后将其内容替换为以下内容：

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

在上面的代码示例中，已定义了一个函数， `square` 该函数采用一个名为的输入 `x` ，并将其与自身相乘。  由于 F # 使用 [类型推理](../language-reference/type-inference.md)，因此 `x` 不需要指定的类型。  F # 编译器了解乘法的有效类型，并根据调用方式将类型分配给 `x` `square` 。  如果将鼠标悬停在上方 `square` ，应会看到以下内容：

```console
val square: x:int -> int
```

这就是所谓函数的类型签名。  可以按如下所示读取： "方形是一个函数，它采用名为 x 的整数并生成一个整数"。  请注意，编译器 `square` 现在提供 `int` 类型，这是因为乘法在 *所有* 类型中都不是泛型的，而是在一组封闭类型中是通用的。  此时将选取 F # 编译器 `int` ，但如果您 `square` 使用其他输入类型（如）调用，则会调整类型签名 `float` 。

定义了另一个函数， `main` 该函数用特性修饰，用 `EntryPoint` 来告知 F # 编译器程序执行应在此开始。  它遵循与其他 [C 样式编程语言](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)相同的约定，可以将命令行参数传递给此函数，并 (通常) 返回整数代码 `0` 。

在此函数中，我们使用的参数调用了 `square` 函数 `12` 。  然后，F # 编译器将的类型分配为 `square` `int -> int` (即，使用 `int` 并生成) 的函数 `int` 。  对的调用 `printfn` 是一个格式化的打印函数，它使用格式字符串（类似于 C 样式编程语言）、与在格式字符串中指定的参数相对应的参数，然后输出结果和新行。

## <a name="running-your-code"></a>运行代码

可以通过单击顶级菜单中的 " **运行** " 来运行代码并查看结果，然后在 **不调试的情况下启动**。  这将在不调试的情况下运行程序，并允许你查看结果。

现在，应会看到以下输出到 Visual Studio for Mac 弹出的控制台窗口：

```console
12 squared is 144!
```

祝贺你！  已在 Visual Studio for Mac 中创建了第一个 F # 项目，并编写了一个 F # 函数，该函数将调用该函数的结果输出，并运行该项目以查看一些结果。

## <a name="using-f-interactive"></a>使用 F# 交互窗口

Visual Studio for Mac Visual F# 工具的最佳功能之一是 F# 交互窗口 "窗口。  它允许你将代码发送到可调用该代码并以交互方式查看结果的进程。

若要开始使用它，请突出显示 `square` 代码中定义的函数。  接下来，单击顶部菜单中的 " **编辑** "。  接下来 **，选择 "发送所选内容" F# 交互窗口**。  这会执行 "F# 交互窗口" 窗口中的代码。  也可以右键单击所选内容，然后选择 " **将所选内容发送到 F# 交互窗口**"。  应会看到 "F# 交互窗口" 窗口中显示以下内容：

```console
>

val square : x:int -> int

>
```

这会显示函数的相同函数签名 `square` ，在您前面悬停该函数时，您会看到此函数签名。  由于 `square` 现在是在 F# 交互窗口进程中定义的，因此可以通过不同的值调用它：

```console
> square 12;;
val it : int = 144
>square 13;;
val it : int = 169
```

这会执行函数，将结果绑定到新名称 `it` ，并显示的类型和值 `it` 。  请注意，必须用终止每行 `;;` 。  这是 F# 交互窗口知道函数调用完成的时间。  您还可以在 F# 交互窗口中定义新函数：

```console
> let isOdd x = x % 2 <> 0;;

val isOdd : x:int -> bool

> isOdd 12;;
val it : bool = false
```

上面定义了一个新函数， `isOdd` 该函数采用 `int` 并检查它是否为奇数！  您可以调用此函数，以查看它如何返回不同的输入。  可以在函数调用中调用函数：

```console
> isOdd (square 15);;
val it : bool = true
```

还可以使用 [管道转发运算符](../language-reference/symbol-and-operator-reference/index.md) 将值传递给两个函数：

```console
> 15 |> square |> isOdd;;
val it : bool = true
```

后面的教程中介绍了管道转发运算符等。

这只是了解到 F# 交互窗口可以执行的操作。  若要了解详细信息，请参阅 [F # 的交互式编程](../tools/fsharp-interactive/index.md)。

## <a name="next-steps"></a>后续步骤

如果你尚未阅读，请查看 [f # 教程](../tour.md)，其中介绍了 f # 语言的一些核心功能。  它将概述 F # 的某些功能，并提供可复制到 Visual Studio for Mac 并运行的丰富代码示例。  在 [F # 指南](../index.yml)中，还可以使用一些很好的外部资源，展示。

## <a name="see-also"></a>请参阅

- [F# 指南](../index.yml)
- [F# 教程](../tour.md)
- [F# 语言参考](../language-reference/index.md)
- [类型推理](../language-reference/type-inference.md)
- [符号和运算符引用](../language-reference/symbol-and-operator-reference/index.md)
