---
title: '通过命令行工具开始处理 F #'
description: 了解如何使用 .NET Core CLI 在任何操作系统 (Windows、macOS 或 Linux) 上构建简单的多项目解决方案。
ms.date: 08/15/2020
ms.openlocfilehash: f890e31fe8c665874dc3034aebfae32e38b9031a
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739910"
---
# <a name="get-started-with-f-with-the-net-core-cli"></a>.NET Core CLI 的 F # 入门

本文介绍如何在 .NET Core CLI (Windows、macOS 或 Linux) 的任何操作系统上开始处理 F #。 其中介绍了如何使用控制台应用程序调用的类库构建多项目解决方案。

## <a name="prerequisites"></a>先决条件

若要开始，必须安装最新 [.NET Core SDK](https://dotnet.microsoft.com/download)。

本文假设你知道如何使用命令行并具有首选文本编辑器。 如果尚未使用该选项， [Visual Studio Code](get-started-vscode.md) 是 F # 的文本编辑器。

## <a name="build-a-simple-multi-project-solution"></a>构建简单的多项目解决方案

打开命令提示符/终端，并使用 [dotnet new](../../core/tools/dotnet-new.md) 命令创建名为的新解决方案文件 `FSNetCore` ：

```dotnetcli
dotnet new sln -o FSNetCore
```

运行上述命令后，将生成以下目录结构：

```console
FSNetCore
    ├── FSNetCore.sln
```

### <a name="write-a-class-library"></a>编写类库

将目录更改为 *FSNetCore*。

使用 `dotnet new` 命令在名为 library 的 **src** 文件夹中创建一个类库项目。

```dotnetcli
dotnet new classlib -lang "F#" -o src/Library
```

运行上述命令后，将生成以下目录结构：

```console
└── FSNetCore
    ├── FSNetCore.sln
    └── src
        └── Library
            ├── Library.fs
            └── Library.fsproj
```

将的内容替换为 `Library.fs` 以下代码：

```fsharp
module Library

open Newtonsoft.Json

let getJsonNetJson value =
    let json = JsonConvert.SerializeObject(value)
    $"I used to be {value} but now I'm {json} thanks to JSON.NET!"
```

将 NuGet 包上的 Newtonsoft.Js添加到库项目。

```dotnetcli
dotnet add src/Library/Library.fsproj package Newtonsoft.Json
```

`Library` `FSNetCore` 使用[dotnet .sln add](../../core/tools/dotnet-sln.md)命令将项目添加到解决方案：

```dotnetcli
dotnet sln add src/Library/Library.fsproj
```

运行 `dotnet build` 以生成项目。 生成时将还原未解析的依赖项。

### <a name="write-a-console-application-that-consumes-the-class-library"></a>编写使用类库的控制台应用程序

使用 `dotnet new` 命令在名为 "应用" 的 **src** 文件夹中创建一个控制台应用程序。

```dotnetcli
dotnet new console -lang "F#" -o src/App
```

运行上述命令后，将生成以下目录结构：

```console
└── FSNetCore
    ├── FSNetCore.sln
    └── src
        ├── App
        │   ├── App.fsproj
        │   ├── Program.fs
        └── Library
            ├── Library.fs
            └── Library.fsproj
```

将 `Program.fs` 文件的内容替换为以下代码：

```fsharp
open System
open Library

[<EntryPoint>]
let main argv =
    printfn "Nice command-line arguments! Here's what JSON.NET has to say about them:"

    for arg in argv do
        let value = getJsonNetJson arg
        printfn $"{value}"

    0 // return an integer exit code
```

`Library`使用[dotnet 添加引用](../../core/tools/dotnet-add-reference.md)添加对项目的引用。

```dotnetcli
dotnet add src/App/App.fsproj reference src/Library/Library.fsproj
```

`App`使用命令将项目添加到 `FSNetCore` 解决方案 `dotnet sln add` ：

```dotnetcli
dotnet sln add src/App/App.fsproj
```

还原 NuGet 依赖项， `dotnet restore` 然后运行 `dotnet build` 以生成项目。

将目录更改为 `src/App` 控制台项目，并运行作为参数传递的项目 `Hello World` ：

```dotnetcli
cd src/App
dotnet run Hello World
```

应该看到以下结果：

```console
Nice command-line arguments! Here's what JSON.NET has to say about them:

I used to be Hello but now I'm ""Hello"" thanks to JSON.NET!
I used to be World but now I'm ""World"" thanks to JSON.NET!
```

## <a name="next-steps"></a>后续步骤

接下来，请查看 [F # 教程](../tour.md) ，了解有关不同 F # 功能的详细信息。
