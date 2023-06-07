---
title: dotnet list reference 命令
description: dotnet list reference 命令可便于列出项目间引用。
ms.date: 02/14/2020
ms.openlocfilehash: b9b34c17102c6218f3d99f6e2620e99f70140284
ms.sourcegitcommit: d223616e7e6fe2139079052e6fcbe25413fb9900
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83802754"
---
# <a name="dotnet-list-reference"></a>dotnet list reference

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet list reference` - 列出项目到项目引用。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet list [<PROJECT>] reference

dotnet list -h|--help
```

## <a name="description"></a>描述

使用 `dotnet list reference` 命令可方便地列出给定项目的项目引用。

## <a name="arguments"></a>自变量

* **`PROJECT`**

  要操作的项目文件。 如果未指定文件，此命令会搜索当前目录来获取文件。

## <a name="options"></a>选项

* **`-h|--help`**

  打印出有关命令的简短帮助。

## <a name="examples"></a>示例

* 列出指定项目的项目引用：

  ```dotnetcli
  dotnet list app/app.csproj reference
  ```

* 列出当前目录中的项目的项目引用：

  ```dotnetcli
  dotnet list reference
  ```
