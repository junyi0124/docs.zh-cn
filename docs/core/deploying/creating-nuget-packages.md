---
title: 使用 .NET Core CLI 创建 NuGet 包
description: 了解如何使用“dotnet pack”命令创建 NuGet 包。
author: cartermp
ms.date: 06/20/2016
ms.technology: dotnet-cli
ms.openlocfilehash: 3f8e75a501cfc48e1c416f71e91290cab1a4ffae
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "76920924"
---
# <a name="how-to-create-a-nuget-package-with-the-net-core-cli"></a>如何使用 .NET Core CLI 创建 NuGet 包

> [!NOTE]
> 下例是关于使用 Unix 的命令行。 此处显示的 `dotnet pack` 命令工作方式与在 Windows 上相同。

对于 .NET Standard 和 .NET Core，所有库都应以 NuGet 包方式发布。 实际上，这是所有 .NET 标准库的发布和使用方式。 可以使用 `dotnet pack` 命令轻松实现此操作。

假设你刚编写了一个很棒的新库，并想通过 NuGet 发布。 你就可以使用跨平台工具创建一个 NuGet 包，完全照做就行！ 下例假定使用一个名为“SuperAwesomeLibrary”  的库，该库以 `netstandard1.0` 为目标。

如果存在可传递的依赖项，也就是说，如果一个项目依赖于另一个包，则在创建 NuGet 包前，确保使用 `dotnet restore` 命令还原整个解决方案的包。 否则将导致 `dotnet pack` 命令不能正常运行。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

确认包已还原后，可以导航到库所在的目录：

```console
cd src/SuperAwesomeLibrary
```

然后，只需在命令行中输入一个命令：

```dotnetcli
dotnet pack
```

/bin/Debug 文件夹现在如下所示  ：

```console
$ ls bin/Debug
netstandard1.0/
SuperAwesomeLibrary.1.0.0.nupkg
SuperAwesomeLibrary.1.0.0.symbols.nupkg
```

这将生成能够进行调试的包。 如果想要生成二进制文件版本的 NuGet 包，只需添加 `--configuration`（或`-c`）开关并使用 `release` 作为参数。

```dotnetcli
dotnet pack --configuration release
```

/bin 文件夹现在将包含一个 release 文件夹，后者包含的 NuGet 包为进制文件版本   ：

```console
$ ls bin/release
netstandard1.0/
SuperAwesomeLibrary.1.0.0.nupkg
SuperAwesomeLibrary.1.0.0.symbols.nupkg
```

因此，现在可以使用此必需的文件发布 NuGet 包了！

## <a name="dont-confuse-dotnet-pack-with-dotnet-publish"></a>不要混淆 `dotnet pack` 和 `dotnet publish`

务必注意，不是任何时候都涉及 `dotnet publish` 命令。 `dotnet publish` 命令用于在同一个包中部署具有所有依赖项的应用程序 - 而不是用于生成通过 NuGet 发布和使用的 NuGet 包。

## <a name="see-also"></a>另请参阅

- [快速入门：创建和发布包](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)
