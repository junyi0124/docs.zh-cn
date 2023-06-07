---
title: 在 SLES 上安装 .NET - .NET
description: 演示在 SLES 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: f351a9b11ab16910963a1db88d88b6949b56ae11
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031795"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-sles"></a>在 SLES 上安装 .NET SDK 或 .NET 运行时

SLES 支持 .NET。 本文介绍如何在 SLES 上安装 .NET。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="supported-distributions"></a>支持的发行版

下表列出了 SLES 12 SP2 和 SLES 15 上当前受支持的 .NET 版本。 这些版本在 [.NET 达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 SLES 版本不再受到支持之前仍受支持。

- ✔️ 指示 SLES 或 .NET 版本仍受支持。
- ❌ 指示 SLES 或 .NET 版本在该 SLES 版本上不受支持。
- 当 SLES 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| SLES                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|------------------------|---------------|---------------|----------------|
| ✔️ [15](#sles-15-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [12 SP2](#sles-12-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

以下 .NET Core 版本不再受支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="remove-preview-versions"></a>删除预览版本

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="how-to-install-other-versions"></a>如何安装其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="sles-15-"></a>SLES 15 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm
```

目前，SLES 15 Microsoft 存储库安装包会将 microsoft-prod.repo 文件安装到错误的目录，从而导致 zypper 找不到 .NET 包。 若要解决此问题，请在正确的目录中创建一个符号链接。

```bash
sudo ln -s /etc/yum.repos.d/microsoft-prod.repo /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="sles-12-"></a>SLES 12 ✔️

SLES 12 系列的 .NET 需要至少为 SP2。

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="troubleshoot-the-package-manager"></a>包管理器疑难解答

本部分提供有关使用包管理器安装 .NET 时可能会遇到的常见错误的信息。

### <a name="failed-to-fetch"></a>未能提取

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a>依赖项

使用包管理器进行安装时，将为你安装这些库。 但是，如果手动安装 .NET 或发布自包含的应用，则需要确保已安装以下库：

- krb5
- libicu
- libopenssl1_1

如果目标运行时环境的 OpenSSL 版本为1.1 或更高版本，则需要安装 compat-openssl10。

有关依赖项的详细信息，请参阅[独立式 Linux 应用](https://github.com/dotnet/core/blob/master/Documentation/self-contained-linux-apps.md)。

对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：

- [libgdiplus（版本 6.0.1 或更高版本）](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > 可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。 有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。

## <a name="scripted-install"></a>脚本安装

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手动安装

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>后续步骤

- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
