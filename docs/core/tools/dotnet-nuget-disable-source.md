---
title: dotnet nuget disable source 命令
description: dotnet nuget disable source 命令在 NuGet 配置文件中禁用现有源。
ms.date: 03/20/2020
ms.openlocfilehash: af37a6589cebaf0501ee5647ebadb83125d0f56e
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537946"
---
# <a name="dotnet-nuget-disable-source"></a>dotnet nuget disable source

**本文适用于：** ✔️ .NET Core 3.1.200 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet nuget disable source` - 禁用 NuGet 源。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget disable source <NAME> [--configfile <FILE>]

dotnet nuget disable source -h|--help
```

## <a name="description"></a>描述

`dotnet nuget disable source` 命令在 NuGet 配置文件中禁用现有源。

## <a name="arguments"></a>自变量

- **`NAME`**

  源的名称。

## <a name="options"></a>选项

- **`--configfile <FILE>`**

  NuGet 配置文件。 如果指定，则只使用此文件中的设置。 如果不指定，将使用当前目录中的配置文件的层次结构。 有关详细信息，请参阅[常见的 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)。

## <a name="examples"></a>示例

- 禁用名为 `mySource` 的源：

  ```dotnetcli
  dotnet nuget disable source mySource
  ```

## <a name="see-also"></a>请参阅

- [NuGet.config 文件中的包源部分](/nuget/reference/nuget-config-file#package-source-sections)

- [sources 命令 (nuget.exe)](/nuget/reference/cli-reference/cli-ref-sources)
