---
title: Visual Studio 中F#的入门
description: 了解如何与 Visual F# Studio 配合使用。
ms.date: 12/20/2019
ms.openlocfilehash: 9bf47fb08ea3df0aa0d5064043d94f030d45d568
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337351"
---
# <a name="get-started-with-f-in-visual-studio"></a>Visual Studio 中F#的入门

F#visual Studio 集成F#开发环境（IDE）中支持可视化工具。

若要开始，请确保已[安装 Visual Studio 并F#提供支持](install-fsharp.md#install-f-with-visual-studio)。

## <a name="create-a-console-application"></a>创建控制台应用程序

在 Visual Studio 中，最基本的项目之一是控制台应用。 下面介绍如何创建主目标服务器：

1. 打开 Visual Studio 2019。

2. 在“开始”窗口上，选择“创建新项目”。

3. 在 "**创建新项目**" 页上， **F#** 从 "语言" 列表中选择。

4. 选择 "**控制台应用（.Net Core）** " 模板，然后选择 "**下一步**"。

5. 在 "**配置新项目**" 页上的 "**项目名称**" 框中输入一个名称。 然后，选择“创建”。

   Visual Studio 将创建新F#项目。 可以在 "解决方案资源管理器" 窗口中查看。

## <a name="write-the-code"></a>编写代码

让我们开始编写一些代码。 确保 `Program.fs` 文件处于打开状态，然后将其内容替换为以下内容：

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

前面的代码示例定义一个名为 `square` 的函数，该函数采用名为 `x` 的输入，并将其与自身相乘。 由于F#使用[类型推理](../language-reference/type-inference.md)，因此不需要指定 `x` 的类型。 F#编译器了解乘法的有效类型，并根据调用 `square` 的方式将类型分配给 `x`。 如果将鼠标悬停在 `square`上，应会看到以下内容：

```fsharp
val square: x:int -> int
```

这就是所谓函数的类型签名。 可按如下所示读取： "方形是一个采用名为 x 的整数的函数，并产生一个整数"。 现在，编译器为 `square` 提供 `int` 类型;这是因为乘法在*所有*类型中都是泛型的，而不是一组封闭类型。 如果F#使用不同的输入类型（如 `float`）调用 `square`，编译器将调整类型签名。

定义了另一个函数 `main`，它是通过 `EntryPoint` 特性修饰的。 此属性告知编译器F#程序执行应在此开始。 它遵循与其他[C 样式编程语言](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)相同的约定，可以将命令行参数传递给此函数，并且返回整数代码（通常 `0`）。

它在入口点函数 `main`中，使用 `12`参数调用 `square` 函数。 然后F# ，编译器分配要 `int -> int` 的 `square` 的类型（即采用 `int` 并生成 `int`的函数）。 对 `printfn` 的调用是一个格式化的打印函数，该函数使用格式字符串并输出结果（和新行）。 格式字符串与 C 样式编程语言类似，具有对应于传递给它的参数（在本例中为 `12` 和 `(square 12)`）的参数（`%d`）。

## <a name="run-the-code"></a>运行代码

可以通过按**Ctrl**+**F5**来运行代码并查看结果。 或者，你可以从顶级菜单栏中选择 "**调试**" > "**启动而不调试**"。 这将运行程序而不进行调试。

以下输出将打印到 Visual Studio 打开的控制台窗口：

```console
12 squared is: 144!
```

祝贺你！ 您已在 Visual Studio F#中创建了第一个项目， F#编写了计算和打印值的函数，并运行该项目以查看结果。

## <a name="next-steps"></a>后续步骤

如果你尚未这样做，请查看[的F#教程](../tour.md)，其中介绍了该F#语言的一些核心功能。 它概述了F#和足够的代码示例的一些功能，你可以将其复制到 Visual Studio 中并运行。

> [!div class="nextstepaction"]
> [F# 教程](../tour.md)

## <a name="see-also"></a>另请参阅

- [F#语言参考](../language-reference/index.md)
- [类型推理](../language-reference/type-inference.md)
- [符号和运算符引用](../language-reference/symbol-and-operator-reference/index.md)
