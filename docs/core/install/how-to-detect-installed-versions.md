---
title: 检查 Windows、Linux 和 macOS 上安装的 .NET 版本 - .NET
description: 了解如何列出计算机上安装的 .NET 版本。 这包括 .NET 运行时和 SDK。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.custom: updateeachrelease
zone_pivot_groups: operating-systems-set-one
ms.openlocfilehash: 2fc12c8c398b1a74d623e53884df666f4d4b85f1
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851609"
---
# <a name="how-to-check-that-net-is-already-installed"></a>如何检查是否已安装 .NET

本文介绍如何检查计算机上安装的 .NET 运行时和 SDK 的版本。 如果你拥有一个集成开发环境（如 Visual Studio 或 Visual Studio for Mac），则可能已安装 .NET。

安装 SDK 便会安装相应的运行时。

如果本文中的任何命令失败，则未安装运行时或 SDK。 有关详细信息，请参阅 [Windows](windows.md)、[macOS](macos.md) 或 [Linux](linux.md) 的安装文章。

## <a name="check-sdk-versions"></a>检查 SDK 版本

可使用终端查看当前安装的 .NET SDK 版本。 打开终端并运行以下命令。

```dotnetcli
dotnet --list-sdks
```

将获得类似于下面的输出。

::: zone pivot="os-windows"

```console
2.1.500 [C:\program files\dotnet\sdk]
2.1.502 [C:\program files\dotnet\sdk]
2.1.504 [C:\program files\dotnet\sdk]
2.1.600 [C:\program files\dotnet\sdk]
2.1.602 [C:\program files\dotnet\sdk]
3.1.100 [C:\program files\dotnet\sdk]
5.0.100 [C:\program files\dotnet\sdk]
```

::: zone-end

::: zone pivot="os-linux"

```bash
2.1.500 [/home/user/dotnet/sdk]
2.1.502 [/home/user/dotnet/sdk]
2.1.504 [/home/user/dotnet/sdk]
2.1.600 [/home/user/dotnet/sdk]
2.1.602 [/home/user/dotnet/sdk]
3.1.100 [/home/user/dotnet/sdk]
5.0.100 [/home/user/dotnet/sdk]
```

::: zone-end

::: zone pivot="os-macos"

```bash
2.1.500 [/usr/local/share/dotnet/sdk]
2.1.502 [/usr/local/share/dotnet/sdk]
2.1.504 [/usr/local/share/dotnet/sdk]
2.1.600 [/usr/local/share/dotnet/sdk]
2.1.602 [/usr/local/share/dotnet/sdk]
3.1.100 [/usr/local/share/dotnet/sdk]
5.0.100 [/usr/local/share/dotnet/sdk]
```

::: zone-end

## <a name="check-runtime-versions"></a>检查运行时版本

可使用以下命令查看当前安装的 .NET 运行时版本。

```dotnetcli
dotnet --list-runtimes
```

将获得类似于下面的输出。

::: zone pivot="os-windows"

```console
Microsoft.AspNetCore.All 2.1.7 [c:\program files\dotnet\shared\Microsoft.AspNetCore.All]
Microsoft.AspNetCore.All 2.1.13 [c:\program files\dotnet\shared\Microsoft.AspNetCore.All]
Microsoft.AspNetCore.App 2.1.7 [c:\program files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 2.1.13 [c:\program files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 3.1.0 [c:\program files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 5.0.0 [c:\program files\dotnet\shared\Microsoft.AspNetCore.App]
Microsoft.NETCore.App 2.1.7 [c:\program files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 2.1.13 [c:\program files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 3.1.0 [c:\program files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.NETCore.App 5.0.0 [c:\program files\dotnet\shared\Microsoft.NETCore.App]
Microsoft.WindowsDesktop.App 3.0.0 [c:\program files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 3.1.0 [c:\program files\dotnet\shared\Microsoft.WindowsDesktop.App]
Microsoft.WindowsDesktop.App 5.0.0 [c:\program files\dotnet\shared\Microsoft.WindowsDesktop.App]
```

::: zone-end

::: zone pivot="os-linux"

```bash
Microsoft.AspNetCore.All 2.1.7 [/home/user/dotnet/shared/Microsoft.AspNetCore.All]
Microsoft.AspNetCore.All 2.1.13 [/home/user/dotnet/shared/Microsoft.AspNetCore.All]
Microsoft.AspNetCore.App 2.1.7 [/home/user/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 2.1.13 [/home/user/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 3.1.0 [/home/user/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 5.0.0 [/home/user/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.NETCore.App 2.1.7 [/home/user/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 2.1.13 [/home/user/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 3.1.0 [/home/user/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 5.0.0 [/home/user/dotnet/shared/Microsoft.NETCore.App]
```

::: zone-end

::: zone pivot="os-macos"

```bash
Microsoft.AspNetCore.All 2.1.7 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.All]
Microsoft.AspNetCore.All 2.1.13 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.All]
Microsoft.AspNetCore.App 2.1.7 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 2.1.13 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 3.1.0 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.AspNetCore.App 5.0.0 [/usr/local/share/dotnet/shared/Microsoft.AspNetCore.App]
Microsoft.NETCore.App 2.1.7 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 2.1.13 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 3.1.0 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]
Microsoft.NETCore.App 5.0.0 [/usr/local/share/dotnet/shared/Microsoft.NETCore.App]
```

::: zone-end

## <a name="check-for-install-folders"></a>检查安装文件夹

可以安装 .NET，但不能将其添加到操作系统或用户配置文件的 `PATH` 变量。 在这种情况下，前面几节中的命令可能不起作用。 作为替代方法，可以检查 .NET 安装文件夹是否存在。

从安装程序或脚本安装 .NET 时，会将其安装到标准文件夹中。 大多数时候，安装 .NET 时使用的安装程序或脚本会让你选择安装到另一个文件夹。 如果选择安装到其他文件夹，请调整文件夹路径的开头。

::: zone pivot="os-windows"

- dotnet executable\
C:\\program files\\dotnet\\dotnet.exe

- **.NET SDK**\
C:\\program files\\dotnet\\sdk\\{version}\\

- .NET Runtime\
C:\\program files\\dotnet\\shared\\{runtime-type}\\{version}\\

::: zone-end

::: zone pivot="os-linux"

- dotnet executable\
/home/user/share/dotnet/dotnet

- **.NET SDK**\
/home/user/share/dotnet/sdk/{version}/

- .NET Runtime\
/home/user/share/dotnet/shared/{runtime-type}/{version}/

::: zone-end

::: zone pivot="os-macos"

- dotnet executable\
/usr/local/share/dotnet/dotnet

- **.NET SDK**\
/usr/local/share/dotnet/sdk/{version}/

- .NET Runtime\
/usr/local/share/dotnet/shared/{runtime-type}/{version}/

::: zone-end

## <a name="more-information"></a>详细信息

可通过命令 `dotnet --info` 查看 SDK 版本和运行时版本。 你还将获得其他环境相关信息，如操作系统版本和运行时标识符 (RID)。

## <a name="next-steps"></a>后续步骤

- [安装 .NET 运行时和适用于 Windows 的 SDK](windows.md)。
- [安装 .NET 运行时和适用于 macOS 的 SDK](macos.md)。
- [安装 .NET 运行时和适用于 Linux 的 SDK](linux.md)。

## <a name="see-also"></a>请参阅

- [确定已安装的 .NET Framework 版本](../../framework/migration-guide/how-to-determine-which-versions-are-installed.md)
