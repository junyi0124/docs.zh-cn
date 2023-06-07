---
title: dotnet remove package 命令
description: dotnet remove package 命令可便于删除对项目的 NuGet 包引用。
ms.date: 02/14/2020
ms.openlocfilehash: fc74ac1364a0ed027b83dab270d382f238dc00e5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2020
ms.locfileid: "81463460"
---
# <a name="dotnet-remove-package"></a>dotnet remove package

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet remove package` - 从项目文件删除包引用。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet remove [<PROJECT>] package <PACKAGE_NAME>

dotnet remove package -h|--help
```

## <a name="description"></a>说明

使用 `dotnet remove package` 命令可方便地从项目删除 NuGet 包引用。

## <a name="arguments"></a>参数

`PROJECT`

指定项目文件。 如果未指定，此命令会搜索当前目录来获取一个项目文件。

`PACKAGE_NAME`

要删除的包引用。

## <a name="options"></a>选项

- **`-h|--help`**

  打印出有关命令的简短帮助。

## <a name="examples"></a>示例

- 从当前目录中的项目删除 `Newtonsoft.Json` NuGet 包：

  ```dotnetcli
  dotnet remove package Newtonsoft.Json
  ```
