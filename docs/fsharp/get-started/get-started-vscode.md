---
title: Visual Studio Code 中的 F# 入门
description: '了解如何将 F # 与 Visual Studio Code 和 Ionide 入门插件套件一起使用。'
ms.date: 12/23/2018
ms.openlocfilehash: 11fb0d443fb7c2b3f270d45bfeaa91102ba28efd
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739797"
---
# <a name="get-started-with-f-in-visual-studio-code"></a>Visual Studio Code 中的 F# 入门

您可以使用[ionide 入门插件](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp)在[Visual Studio Code](https://code.visualstudio.com)中编写 F #，以利用 IntelliSense 和代码重构 (IDE) 体验。 请访问 [Ionide.io](https://ionide.io) ，了解有关该插件的详细信息。

若要开始，请确保已 [正确安装 F # 和 ionide 入门插件](install-fsharp.md#install-f-with-visual-studio-code)。

## <a name="create-your-first-project-with-ionide"></a>通过 Ionide 入门创建第一个项目

若要创建新的 F # 项目，请打开命令行并使用 .NET Core CLI 创建新项目：

```dotnetcli
dotnet new console -lang "F#" -o FirstIonideProject
```

完成后，将目录更改为该项目并打开 Visual Studio Code：

```console
cd FirstIonideProject
code .
```

在 Visual Studio Code 上加载项目后，会在窗口的左侧看到 F # 解决方案资源管理器窗格处于打开状态。 这意味着 Ionide 入门已成功加载您刚刚创建的项目。 您可以在此时间点之前在编辑器中编写代码，但一旦发生这种情况，就会完成加载。

## <a name="configure-f-interactive"></a>配置 F # interactive

首先，请确保 .NET Core 脚本是您的默认脚本环境：

1. 打开 Visual Studio Code 设置 (**代码**  >  **首选项**  >  **设置**) 。
1. 搜索术语 " **F # 脚本**"。
1. 单击 " **fsharp.core：使用 SDK 脚本**" 复选框。

目前这是必需的，因为在基于 .NET Framework 的脚本中，某些旧行为不能用于 .NET Core 脚本，Ionide 入门目前正在努力实现这种向后兼容性。 未来，.NET Core 脚本将成为默认值。

### <a name="write-your-first-script"></a>编写你的第一个脚本

将 Visual Studio Code 配置为使用 .NET Core 脚本后，请导航到 Visual Studio Code 中的资源管理器视图，并创建新文件。 将其命名为 *MyFirstScript. .fsx*。

现在，将以下代码添加到其中：

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx)]

此函数将字转换为 [Pig 拉丁](https://en.wikipedia.org/wiki/Pig_Latin)形式的单词。 下一步是使用 F# 交互窗口 (FSI.EXE) 对其进行评估。

突出显示整个函数 (其长度应为11行) 。 突出显示后，请按住 **Alt** 键，然后按 **Enter**。 你会注意到，屏幕底部会弹出一个终端窗口，其外观应如下所示：

![Ionide 入门的 F# 交互窗口输出示例](./media/getting-started-vscode/vscode-fsi.png)

这三个方面：

1. 它启动了 FSI.EXE 进程。
2. 它发送了你在 FSI.EXE 过程中突出显示的代码。
3. FSI.EXE 进程对你发送的代码进行评估。

由于发送的内容是一个 [函数](../language-reference/functions/index.md)，因此你现在可以使用 fsi.exe 调用该函数！ 在交互窗口中，键入以下内容：

```fsharp
toPigLatin "banana";;
```

应该会看到以下结果：

```fsharp
val it : string = "ananabay"
```

现在，让我们尝试使用元音作为第一个字母。 输入以下内容：

```fsharp
toPigLatin "apple";;
```

应该会看到以下结果：

```fsharp
val it : string = "appleyay"
```

函数看起来像预期那样工作。 恭喜，你只是在 Visual Studio Code 中编写了第一个 F # 函数，并通过 FSI.EXE 对其进行了评估！

> [!NOTE]
> 正如您可能已经注意到的，FSI.EXE 中的行以结尾 `;;` 。 这是因为 FSI.EXE 允许输入多个行。 `;;`结束时，fsi.exe 知道代码的完成时间。

## <a name="explaining-the-code"></a>解释代码

如果你不确定代码实际执行的操作，以下是一个循序渐进的步骤。

正如您所看到的，它 `toPigLatin` 是一个将单词作为其输入并将其转换为该单词 Pig-Latin 表示形式的函数。 此操作的规则如下所示：

如果单词中的第一个字符以元音开头，则将 "yay" 添加到单词的结尾。 如果它不以元音开头，请将第一个字符移动到单词末尾，并向其添加 "ay"。

您可能已注意到 FSI.EXE 中的以下内容：

```fsharp
val toPigLatin : word:string -> string
```

这表明， `toPigLatin` `string` 作为输入 (进入 `word`) 并返回另一个函数 `string` 。 这称为 [函数的类型签名](https://fsharpforfunandprofit.com/posts/function-signatures/)，这是 f # 的一小部分，它是了解 f # 代码的关键部分。 如果将鼠标悬停在 Visual Studio Code 的函数上，也会注意到这一点。

在函数体中，你会注意到两个不同的部分：

1. 一个名为的内部函数， `isVowel` 通过检查是否将给定的字符 (`c`) 为元音，通过 [模式匹配](../language-reference/pattern-matching.md)进行检查是否匹配提供的模式之一：

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L2-L6)]

2. 一个 [`if..then..else`](../language-reference/conditional-expressions-if-then-else.md) 表达式，用于检查第一个字符是否为元音，并根据第一个字符是否为元音，从输入字符中构造返回值：

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L8-L11)]

这样的流 `toPigLatin` 就是：

检查输入词的第一个字符是否为元音。 如果是，请将 "yay" 附加到单词的结尾。 否则，将第一个字符移动到单词末尾，并向其添加 "ay"。

最后要注意的一点是：在 F # 中，没有从函数返回的显式指令。 这是因为 F # 是基于表达式的，并且在函数主体中计算的最后一个表达式确定该函数的返回值。 因为 `if..then..else` 本身是一个表达式，所以块体的计算 `then` 或块体的计算确定了 `else` 函数返回的值 `toPigLatin` 。

## <a name="turn-the-console-app-into-a-pig-latin-generator"></a>将控制台应用程序转换为 Pig 拉丁语生成器

本文前面的部分演示了编写 F # 代码的常见第一步：编写一个初始函数并使用 FSI.EXE 以交互方式执行该函数。 这称为 "复制驱动开发"，其中，" [复制](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) " 代表 "读取-评估-打印循环"。 这是一种非常好的方法，可让你体验功能。

复制驱动开发的下一步是将工作代码移到 F # 实现文件中。 然后，它可以由 F # 编译器编译为可执行的程序集。

若要开始，请打开先前用 .NET Core CLI 创建的 *程序. fs* 文件。 你会注意到，其中已存在某些代码。

接下来，创建一个名为的新 [`module`](../language-reference/modules.md) `PigLatin` 函数，并将 `toPigLatin` 前面创建的函数复制到其中：

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/pig-latin.fs#L3-L14)]

此模块应位于函数的上方 `main` 和声明的下方 `open System` 。 在 F # 中，声明的顺序很重要，因此在文件中调用函数之前，需要先定义函数。

现在，在 `main` 函数中，对参数调用 Pig 拉丁语生成器函数：

```fsharp
[<EntryPoint>]
let main argv =
    for name in argv do
        let newName = PigLatin.toPigLatin name
        printfn %"{name} in Pig Latin is: {newName}"

    0
```

现在，你可以从命令行运行控制台应用：

```dotnetcli
dotnet run apple banana
```

您将看到，它输出的结果与您的脚本文件的结果相同，但这一次是正在运行的程序！

## <a name="troubleshooting-ionide"></a>Ionide 入门故障排除

可以通过以下几种方法来解决你可能遇到的某些问题：

1. 若要获取 Ionide 入门的代码编辑功能，需要将 F # 文件保存到磁盘，并将其保存到在 Visual Studio Code 工作区中打开的文件夹中。
1. 如果已对系统进行了更改或安装了 Visual Studio Code 的 Ionide 入门必备组件，请 Visual Studio Code 重新启动。
1. 如果项目目录中包含无效字符，则 Ionide 入门可能不起作用。  如果出现这种情况，请重命名项目目录。
1. 如果所有 Ionide 入门命令都不起作用，请检查 [Visual Studio Code 键绑定](https://code.visualstudio.com/docs/getstarted/keybindings#_advanced-customization) ，以确定是否会意外地替代它们。
1. 如果计算机上的 Ionide 入门中断，并且上述任何内容都不能解决问题，请尝试删除 `ionide-fsharp` 计算机上的目录，并重新安装插件套件。
1. 如果项目未能加载 (F # 解决方案资源管理器将显示此) ，请右键单击该项目，然后单击 " **查看详细信息** " 以获取更多诊断信息。

Ionide 入门是由 F # 社区成员生成和维护的开源项目。 请在 [ionide 入门-vscode-Fsharp.core GitHub 存储库](https://github.com/ionide/ionide-vscode-fsharp)上报告问题，并将其提供给你。

你还可以在 [Ionide 入门 Gitter 通道](https://gitter.im/ionide/ionide-project)中请求 ionide 入门开发人员和 F # 社区提供进一步的帮助。

## <a name="next-steps"></a>后续步骤

若要了解有关 F # 和语言功能的详细信息，请参阅 [f # 教程](../tour.md)。
