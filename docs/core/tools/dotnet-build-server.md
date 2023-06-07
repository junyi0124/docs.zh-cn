---
title: dotnet build-server 命令
description: dotnet build-server 命令与通过生成启动的服务器进行交互。
ms.date: 02/14/2020
ms.openlocfilehash: 882b697c07aac0e20266f3ad4e6c11888a0b7acc
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463726"
---
# <a name="dotnet-build-server"></a>dotnet build-server

 本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>名称

`dotnet build-server` -与通过生成启动的服务器进行交互。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet build-server shutdown [--msbuild] [--razor] [--vbcscompiler]

dotnet build-server shutdown -h|--help

dotnet build-server -h|--help
```

## <a name="commands"></a>命令

- **`shutdown`**

  关闭从 dotnet 启动的生成服务器。 默认情况下会关闭所有服务器。

## <a name="options"></a>选项

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--msbuild`**

  关闭 MSBuild 生成服务器。

- **`--razor`**

  关闭 Razor 生成服务器。

- **`--vbcscompiler`**

  关闭 VB/C# 编译器生成服务器。
