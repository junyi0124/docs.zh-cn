---
title: 在 Ubuntu 上安装 .NET - .NET
description: 演示在 Ubuntu 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 22ce3379e028f065528e1f507a2d8c1ae598f0e8
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031836"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-ubuntu"></a>在 Ubuntu 上安装 .NET SDK 或 .NET 运行时

Ubuntu 支持 .NET。 本文介绍如何在 Ubuntu 上安装 .NET。 如果 Ubuntu 版本不受支持，则该版本不再支持 .NET。 不过，可以按照这些说明在这些版本上运行 .NET，即使它不受支持。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a>支持的分发

下表列出了当前支持的 .NET 版本以及支持它们的 Ubuntu 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Ubuntu 的版本达到生命周期](https://wiki.ubuntu.com/Releases)之前仍受支持。

- ✔️ 指示 Ubuntu 或 .NET 版本仍受支持。
- ❌ 指示 Ubuntu 或 .NET 版本在该 Ubuntu 版本上不受支持。
- 当 Ubuntu 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Ubuntu                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [20.10](#2010-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [20.04 (LTS)](#2004-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [19.10](#1910-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [19.04](#1904-)       | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [18.10](#1810-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ✔️ [18.04 (LTS)](#1804-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [17.10](#1710-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ❌ [17.04](#1704-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ❌ [16.10](#1610-)       | ❌ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ✔️ [16.04 (LTS)](#1604-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="remove-preview-versions"></a>删除预览版本

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="how-to-install-other-versions"></a>如何安装其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="2010-"></a>20.10 ✔️

> [!IMPORTANT]
> .NET Core 2.1 在包源中尚不可用。

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/20.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="2004-"></a>20.04 ✔️

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="1910-"></a>19.10 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1904-"></a>19.04 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-31](includes/linux-install-31-apt.md)]

## <a name="1810-"></a>18.10 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1804-"></a>18.04 ✔️

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="1710-"></a>17.10 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1704-"></a>17.04 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/17.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1610-"></a>16.10 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-ubuntu.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

## <a name="1604-"></a>16.04 ✔️

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="apt-update-sdk-or-runtime"></a>APT 更新 SDK 或运行时

当新的修补程序版本适用于 .NET 时，只需使用以下命令通过 APT 进行升级：

```bash
sudo apt-get update
sudo apt-get upgrade
```

## <a name="apt-troubleshooting"></a>APT 疑难解答

本部分提供有关使用 APT 安装 .NET 时可能会遇到的常见错误的信息。

### <a name="unable-to-find-package"></a>找不到包

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

### <a name="unable-to-locate--some-packages-could-not-be-installed"></a>找不到 \\ 无法安装某些包

[!INCLUDE [package-manager-failed-to-find-deb](includes/package-manager-failed-to-find-deb.md)]

```bash
sudo apt-get install -y gpg
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor -o microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/ubuntu/{os-version}/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
sudo apt-get update; \
  sudo apt-get install -y apt-transport-https && \
  sudo apt-get update && \
  sudo apt-get install -y {dotnet-package}
```

### <a name="failed-to-fetch"></a>未能提取

[!INCLUDE [package-manager-failed-to-fetch-deb](includes/package-manager-failed-to-fetch-deb.md)]

## <a name="snap"></a>对齐

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a>依赖项

使用包管理器进行安装时，将为你安装这些库。 但是，如果手动安装 .NET 或发布自包含的应用，则需要确保已安装以下库：

- libc6
- libgcc1
- libgssapi-krb5-2
- libicu52（针对 14.x）
- libicu55（针对 16.x）
- libicu60（针对 18.x）
- libicu66（适用于 20. x）
- libssl1.0.0（适用于 14.x、16.x）
- libssl1.1（适用于18.x、20.x）
- libstdc++6
- zlib1g

对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：

- libgdiplus（版本 6.0.1 或更高版本）

  > [!WARNING]
  > 可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。 有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。

## <a name="scripted-install"></a>脚本安装

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手动安装

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>后续步骤

- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
