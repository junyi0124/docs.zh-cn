---
title: dotnet nuget locals 命令
description: dotnet nuget locals 命令可清除或列出本地 NuGet 资源，如 http 请求缓存、临时缓存或整个计算机范围内的全局包文件夹。
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: 5b421b5058528a93c7be58eef2932937cc9cc12d
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463533"
---
# <a name="dotnet-nuget-locals"></a>dotnet nuget locals

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet nuget locals` -清除或列出本地 NuGet 资源。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget locals <CACHE_LOCATION> [(-c|--clear)|(-l|--list)] [--force-english-output]

dotnet nuget locals -h|--help
```

## <a name="description"></a>说明

`dotnet nuget locals` 命令清除或列出 http 请求缓存中的本地 NuGet 资源，临时缓存或计算机范围的全局包文件夹。

## <a name="arguments"></a>参数

- **`CACHE_LOCATION`**

  要列出或清除的缓存位置。 可以接受以下值之一：

  * `all` - 表示指定的操作应用于所有缓存类型，即 http 请求缓存、全局包缓存和临时缓存。
  * `http-cache` - 表示指定的操作仅应用于 http 请求缓存。 其他缓存位置不受影响。
  * `global-packages` - 表示指定的操作仅应用于全局包缓存。 其他缓存位置不受影响。
  * `temp` - 表示指定的操作仅应用于临时缓存。 其他缓存位置不受影响。

## <a name="options"></a>选项

- **`--force-english-output`**

  使用固定的、基于英语的区域性强制运行应用程序。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`-c|--clear`**

  清除选项对指定的缓存类型执行清除操作。 缓存目录的内容被以递归方式删除。 正在执行的用户/组必须具有对缓存目录中的文件的相关权限。 反之，则显示错误，指示未清除的文件/文件夹。

- **`-l|--list`**

  列表选项用于显示指定缓存类型的位置。

## <a name="examples"></a>示例

- 显示所有本地缓存目录的路径（http 缓存目录、全局包缓存目录和临时缓存目录）：

  ```dotnetcli
  dotnet nuget locals all –l
  ```

- 显示本地 http 缓存录的路径：

  ```dotnetcli
  dotnet nuget locals http-cache --list
  ```

- 清除所有本地缓存目录的文件（http 缓存目录、全局包缓存目录和临时缓存目录）：

  ```dotnetcli
  dotnet nuget locals all --clear
  ```

- 清除本地全局包缓存目录中的所有文件：

  ```dotnetcli
  dotnet nuget locals global-packages -c
  ```

- 清除本地临时缓存目录中的所有文件：

  ```dotnetcli
  dotnet nuget locals temp -c
  ```

## <a name="troubleshooting"></a>故障排除

有关使用 `dotnet nuget locals` 命令时的常见问题和错误的信息，请参阅[管理 NuGet 缓存](/nuget/consume-packages/managing-the-nuget-cache)。
