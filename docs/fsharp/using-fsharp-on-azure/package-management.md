---
title: '将包管理与 F # 一起用于 Azure'
description: '使用 Paket 或 NuGet 管理 F # Azure 依赖项'
author: sylvanc
ms.date: 09/20/2016
ms.custom: devx-track-fsharp
ms.openlocfilehash: 7816c82e87db113a35fef967886c8c3e27959332
ms.sourcegitcommit: a8a205034eeffc7c3e1bdd6f506a75b0f7099ebf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91756229"
---
# <a name="package-management-for-f-azure-dependencies"></a>F# Azure 依赖项的包管理

使用包管理器时，可轻松为 Azure 开发获取包。 这两个选项是 [Paket](https://fsprojects.github.io/Paket/) 和 [NuGet](https://www.nuget.org/)。

## <a name="using-paket"></a>使用 Paket

如果使用 [Paket](https://fsprojects.github.io/Paket/) 作为依赖关系管理器，可以使用 `paket.exe` 工具添加 Azure 依赖关系。 例如： 。

```console
> paket add nuget WindowsAzure.Storage
```

或者，如果使用 [Mono](https://www.mono-project.com/) 进行跨平台 .net 开发：

```console
> mono paket.exe add nuget WindowsAzure.Storage
```

这会将添加 `WindowsAzure.Storage` 到当前目录中的项目的包依赖项集，修改该 `paket.dependencies` 文件，然后下载包。 如果以前设置了依赖项，或者正在使用的项目的依赖项已由其他开发人员进行了设置，则可以按以下方式解析和安装依赖项：

```console
> paket install
```

或者，对于 Mono 开发：

```console
> mono paket.exe install
```

可以将所有包依赖项更新到最新版本，如下所示：

```console
> paket update
```

或者，对于 Mono 开发：

```console
> mono paket.exe update
```

## <a name="using-nuget"></a>使用 NuGet

如果使用 [NuGet](https://www.nuget.org/) 作为依赖关系管理器，可以使用 `nuget.exe` 工具添加 Azure 依赖关系。 例如： 。

```console
> nuget install WindowsAzure.Storage -ExcludeVersion
```

或者，对于 Mono 开发：

```console
> mono nuget.exe install WindowsAzure.Storage -ExcludeVersion
```

这会将添加 `WindowsAzure.Storage` 到当前目录中的项目的包依赖项集，并下载包。 如果以前设置了依赖项，或者正在使用的项目的依赖项已由其他开发人员进行了设置，则可以按以下方式解析和安装依赖项：

```console
> nuget restore
```

或者，对于 Mono 开发：

```console
> mono nuget.exe restore
```

可以将所有包依赖项更新到最新版本，如下所示：

```console
> nuget update
```

或者，对于 Mono 开发：

```console
> mono nuget.exe update
```

## <a name="referencing-assemblies"></a>引用程序集

若要在 F # 脚本中使用包，需要使用指令引用包中包含的程序集 `#r` 。 例如： 。

```fsharp
> #r "packages/WindowsAzure.Storage/lib/net40/Microsoft.WindowsAzure.Storage.dll"
```

如您所见，您需要指定 DLL 的相对路径和完整的 DLL 名称，这些路径可能与包名称不完全相同。 该路径将包括一个框架版本，还可能包含一个包版本号。 若要查找所有已安装的程序集，可以在 Windows 命令行中使用如下所示的内容：

```console
> cd packages/WindowsAzure.Storage
> dir /s/b *.dll
```

或在 Unix shell 中，如下所示：

```console
> find packages/WindowsAzure.Storage -name "*.dll"
```

此时将显示已安装的程序集的路径。 在该处，你可以为 framework 版本选择正确的路径。
