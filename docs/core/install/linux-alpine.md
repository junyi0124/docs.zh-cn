---
title: 在 Alpine 上安装 .NET - .NET
description: 演示在 Alpine 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 29901cc24ddd4bbe8200a36765ddd29f501394c0
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506793"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-alpine"></a>在 Alpine 上安装 .NET SDK 或 .NET 运行时

本文介绍如何在 Alpine 上安装 .NET。 如果 Alpine 版本不再受到支持，则该版本不再支持 .NET。 不过，可以按照这些说明在这些版本上运行 .NET，即使它不受支持。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

Alpine 没有安装程序。 必须使用[安装脚本](#scripted-install)或按照[手动安装](#manual-install)说明进行操作。

## <a name="supported-distributions"></a>支持的发行版

下表列出了当前支持的 .NET 版本以及支持它们的 Alpine 版本。 这些版本在 [.NET 到达支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Alpine 的版本到达有效期](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases)之前仍受支持。

- ✔️ 指示 Alpine 或 .NET 版本仍受支持。
- ❌ 指示 Alpine 或 .NET 版本在该 Alpine 发行版本上不受支持。
- 当 Alpine 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Alpine  | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|-------- |---------------|---------------|----------------|
| ✔️ 3.12 | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ 3.11 | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ 3.10 | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ 3.9  | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ 3.8  | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="dependencies"></a>依赖项

Alpine Linux 上的 .NET 要求安装以下依赖项：

- icu-libs
- krb5-libs
- libgcc
- libintl
- libssl1.1（Alpine v3.9 或更高版本）
- libssl1.0（Alpine v3.8 或更低版本）
- libstdc++
- zlib

## <a name="scripted-install"></a>脚本安装

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手动安装

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>后续步骤

- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
