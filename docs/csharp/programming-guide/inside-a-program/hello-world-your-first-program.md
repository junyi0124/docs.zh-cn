---
title: Hello World - 你在 Windows 或 Mac 上使用 Visual Studio 的第一个程序 - C# 编程指南
description: Visual Studio 是一个专业的开发环境，具有适用于 .NET 开发的许多功能。 使用 Visual Studio 创建 C# 版本的 Hello World！
ms.date: 09/12/2019
f1_keywords:
- cs.program
- vs.csharp.startpage.firstapplication
helpviewer_keywords:
- examples [C#], Hello World
- Hello World example [C#]
ms.assetid: 6493182a-b0b6-4539-a719-518a168cb730
ms.openlocfilehash: b78937b8ba1c7d4718bfb6252dac705945336d2a
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301822"
---
# <a name="hello-world----your-first-program"></a>Hello World -- 你的第一个程序

在本文中，你将使用 Visual Studio 创建传统的“Hello World!” 版本。 Visual Studio 是一个专业的集成开发环境 (IDE)，具有适用于 .NET 开发的许多功能。 只需使用 Visual Studio 中的几个功能即可创建此程序。 如需了解有关 Visual Studio 的详细信息，请参阅 [Visual C# 入门](/visualstudio/ide/quickstart-csharp-console)。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="create-a-new-application"></a>创建一个新的应用程序

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

启动 Visual Studio。 你将在 Windows 上看到以下图像：

![Windows 上的 Visual Studio 欢迎屏幕](./media/hello-world-your-first-program/visual-studio-windows-start-screen.png)

在图像右下角选择“创建新项目”。 Visual Studio 会显示“新建项目”对话框：

![Windows 上的 Visual Studio 新建项目屏幕](./media/hello-world-your-first-program/visual-studio-windows-new-project.png)

> [!NOTE]
> 如果这是首次启动 Visual Studio，则“最近使用的项目模板”列表为空。

在“新建项目”对话框中，选择“控制台应用(.NET Core)”，然后按“下一步”。 请为项目命名（例如“HelloWorld”），然后按“创建”。

Visual Studio 将打开你的项目。 一个基本的“Hello World!”已初见雏形 示例。 按 `Ctrl` + `F5` 运行项目。 Visual Studio 生成项目，将源代码转换为可执行文件。 然后，它会启动一个运行新应用程序的命令窗口。 你应在窗口中看到以下文本：

```console
Hello World!

C:\Program Files\dotnet\dotnet.exe (process 11964) exited with code 0.
Press any key to close this window . . .
```

按相关键关闭窗口。

# <a name="macos"></a>[macOS](#tab/macos)

启动 Visual Studio for Mac。 你将在 Mac 上看到以下图像：

![Mac 上的 Visual Studio 欢迎屏幕](./media/hello-world-your-first-program/visual-studio-mac-start-screen.png)

> [!NOTE]
> 如果这是首次启动 Visual Studio for Mac，则“最近使用的项目模板”列表为空。

在图像右上角选择“新建”。 Visual Studio for Mac 会显示“新建项目”对话框：

![Mac 上的 Visual Studio 新建项目屏幕](./media/hello-world-your-first-program/visual-studio-mac-new-project.png)

在“新建项目”对话框中，选择“.NET Core”和“控制台应用”，然后按“下一步”。 需要选择目标框架。 默认情况下，请按“下一步”。 请为项目命名（例如“HelloWorld”），然后按“创建”。 可以使用默认的项目位置。 请勿将项目添加到源控件。

Visual Studio for Mac 打开你的项目。 一个基本的“Hello World!”已初见雏形 示例。 按 `Ctrl` + `Fn` + `F5` 运行项目。 Visual Studio for Mac 生成项目，将源代码转换为可执行文件。 然后，它会启动一个运行新应用程序的命令窗口。 你应在窗口中看到以下文本：

```console
Hello World!

Press any key to close this window . . .
```

按任意键结束会话。

---

## <a name="elements-of-a-c-program"></a>C# 程序的元素

我们来检查一下此程序的重要部分。 第一行包含一条注释。 字符 `//` 将该行的其余内容转换为一条注释。

[!code-csharp[csProgGuide#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#32)]

还可以通过将文本块置于 `/*` 和 `*/` 字符之间，禁止对其的注释。 这在下面的示例中显示。

[!code-csharp[csProgGuide#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#33)]

C# 控制台应用程序必须包含 `Main` 方法，控件在其中开始和结束。 在 `Main` 方法中，可以创建对象并执行其他方法。

`Main` 方法是驻留在类或结构中的[静态](../../language-reference/keywords/static.md)方法。 在上一个“Hello World!” 示例中，该方法驻留在名为 `Hello` 的类中。 可以通过以下方式之一来声明 `Main` 方法：

- 它可返回 `void`。 这表示程序不会返回值。

[!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

- 还可以返回一个整数。 整数是应用程序的“退出代码”。

[!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

- 无论使用哪种返回类型，它都可以带有参数。

[!code-csharp[csProgGuideMain#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#19)]

\- 或 -

[!code-csharp[csProgGuideMain#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#18)]

`Main` 方法中定义的参数 `args` 是一个 `string` 类型的数组，该数组包含用于调用该程序的命令行自变量。

若要了解更多有关如何使用命令行参数的信息，请参阅 [Main() 和命令行参数](../main-and-command-args/index.md)中的示例。

## <a name="input-and-output"></a>输入和输出

C# 程序通常使用由 .NET 的运行时库提供的输入/输出服务。 语句 `System.Console.WriteLine("Hello World!");` 使用 <xref:System.Console.WriteLine%2A> 方法。 这是运行时库中 <xref:System.Console> 类的输出方法之一。 该方法将在标准输出流中显示其字符串参数，后接新行。 其他 <xref:System.Console> 方法可用于不同的输入和输出操作。 如果程序开头包含 `using System;` 指令，则可以直接使用 <xref:System> 类和方法，而不必进行完全限定。 例如，可以调用 `Console.WriteLine`，而非 `System.Console.WriteLine`：

[!code-csharp[csProgGuide#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/using.cs#1)]

[!code-csharp[csProgGuide#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuide/CS/progGuide.cs#23)]

有关输入/输出方法的详细信息，请参阅 <xref:System.IO>。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [示例和教程](../../../samples-and-tutorials/index.md)
- [Main() 和命令行参数](../main-and-command-args/index.md)
- [Visual C# 入门](/visualstudio/ide/quickstart-csharp-console)
