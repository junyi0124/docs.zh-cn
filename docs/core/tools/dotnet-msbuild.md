---
title: dotnet msbuild 命令
description: dotnet msbuild 命令可提供对 MSBuild 命令行的访问权限。
ms.date: 02/14/2020
ms.openlocfilehash: 9739fe782e17db3955db087ca1781ad4280cd491
ms.sourcegitcommit: 1eae045421d9ea2bfc82aaccfa5b1ff1b8c9e0e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/16/2020
ms.locfileid: "84803177"
---
# <a name="dotnet-msbuild"></a>dotnet msbuild

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet msbuild` - 生成项目及其所有依赖项。 注意：如果有多个解决方案或项目文件，可能需要指定一个。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet msbuild <MSBUILD_ARGUMENTS>

dotnet msbuild -h
```

## <a name="description"></a>描述

`dotnet msbuild` 命令允许访问功能完备的 MSBuild。

该命令与仅适用于 SDK 样式项目的现有 MSBuild 命令行客户端具有完全相同的功能。 选项一致。 有关可用选项的详细信息，请参阅 [MSBuild 命令行参考](/visualstudio/msbuild/msbuild-command-line-reference)。

[dotnet build](dotnet-build.md) 命令相当于 `dotnet msbuild -restore`。 如果不想生成项目，并且拥有要运行的特定目标，请使用 `dotnet build` 或 `dotnet msbuild` 并指定目标。

## <a name="examples"></a>示例

- 生成项目及其依赖项：

  ```dotnetcli
  dotnet msbuild
  ```

- 使用“发布”配置生成项目及其依赖项：

  ```dotnetcli
  dotnet msbuild -property:Configuration=Release
  ```

- 运行发布目标并发布 `osx.10.11-x64` RID：

  ```dotnetcli
  dotnet msbuild -target:Publish -property:RuntimeIdentifiers=osx.10.11-x64
  ```

- 请参阅包含 SDK 添加的所有目标的整个项目：

  ```dotnetcli
  dotnet msbuild -preprocess
  dotnet msbuild -preprocess:<fileName>.xml
  ```
