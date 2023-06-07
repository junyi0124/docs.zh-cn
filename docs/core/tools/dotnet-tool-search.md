---
title: dotnet tool search 命令
description: Dotnet tool search 命令搜索已发布到 NuGet.org 的 .NET 工具。
ms.date: 11/11/2020
ms.openlocfilehash: e0289e651ec4a439c791c8c948bef2d85d9c3794
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634137"
---
# <a name="dotnet-tool-search"></a>dotnet tool search

**本文适用于：** ✔️ .NET 5.0 SDK 及更高版本

## <a name="name"></a>名称

`dotnet tool search` - 搜索已发布到 NuGet 的 [.NET 工具](global-tools.md)。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet tool search [--detail]  [--prerelease]
    [--skip <NUMBER>] [--take <NUMBER>] <SEARCH TERM>

dotnet tool search -h|--help
```

## <a name="description"></a>描述

`dotnet tool search` 命令提供了一种方法，使你可以在 NuGet 中搜索可用作 .NET 全局、工具路径或本地工具的工具。 该命令搜索工具名称和元数据（如标题、说明和标记）。

该命令使用 [NuGet 搜索 API](/nuget/api/search-query-service-resource#search-for-packages)。 它筛选 `packageType=dotnettool` 以仅选择 .NET 工具包。

## <a name="options"></a>选项

- **`--detail`**

  显示查询的详细结果。

- **`--prerelease`**

  包含预发行包。

- **`--skip <NUMBER>`**

  指定要跳过的查询结果数。 用于分页。

- **`--take <NUMBER>`**

  指定要显示的查询结果数。 用于分页。

- **`-h|--help`**

  显示命令行帮助。

## <a name="examples"></a>示例

- 在 NuGet.org 中搜索其包名称或描述中具有“格式”的 .NET 工具：

  ```dotnetcli
  dotnet tool search format
  ```

  输出如下所示：

  ```output
  Package ID                              Latest Version      Authors                                                                     Downloads      Verified
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------
  dotnet-format                           4.1.131201          Microsoft                                                                   496746
  bsoa.generator                          1.0.0               Microsoft                                                                   533
  ```

- 在 NuGet.org 中搜索其包名称或元数据中具有“格式”的 .NET 工具，仅显示第一条结果并显示详细视图。

  ```dotnetcli
  dotnet tool search format --take 1 --detail
  ```

  输出如下所示：

  ```output
  ----------------
  dotnet-format
  Latest Version: 4.1.131201
  Authors: Microsoft
  Tags:
  Downloads: 496746
  Verified: False
  Description: Command line tool for formatting C# and Visual Basic code files based on .editorconfig settings.
  Versions:
          3.0.2 Downloads: 1973
          3.0.4 Downloads: 9064
          3.1.37601 Downloads: 114730
          3.2.107702 Downloads: 13423
          3.3.111304 Downloads: 131195
          4.0.130203 Downloads: 78610
          4.1.131201 Downloads: 145927
  ```

## <a name="see-also"></a>另请参阅

- [.NET 工具](global-tools.md)
- [教程：使用 .NET CLI 安装和使用 .NET 全局工具](global-tools-how-to-use.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
