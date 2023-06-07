---
title: 在 Debian 上安装 .NET - .NET
description: 演示在 Debian 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 683d0a9c47edf3cf9c47426d659e778eeb6f84df
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031886"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-debian"></a>在 Debian 上安装 .NET SDK 或 .NET 运行时

本文介绍如何在 Debian 上安装 .NET。 如果 Debian 版本不受支持，则该版本不再支持 .NET。 不过，可以按照这些说明在这些版本上运行 .NET，即使它不受支持。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a>支持的分发

下表列出了当前支持的 .NET 版本以及支持它们的 Debian 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Debian 的版本达到生命周期](https://wiki.debian.org/DebianReleases)之前仍受支持。

- ✔️ 指示 Debian 或 .NET 版本仍受支持。
- ❌ 指示 Debian 或 .NET 版本在该 Debian 版本上不受支持。
- 当 Debian 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Debian                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [10](#debian-10-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [9](#debian-9-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [8](#debian-8-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="remove-preview-versions"></a>删除预览版本

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="how-to-install-other-versions"></a>如何安装其他版本

[!INCLUDE [hack-pkgname](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="debian-10-"></a>Debian 10 ✔️

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="debian-9-"></a>Debian 9 ✔️

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/9/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```

[!INCLUDE [linux-apt-install-50](includes/linux-install-50-apt.md)]

## <a name="debian-8-"></a>Debian 8 ❌

[!INCLUDE [linux-not-supported](includes/linux-not-supported-debian.md)]

[!INCLUDE [linux-prep-intro-apt](includes/linux-prep-intro-apt.md)]

```bash
wget -O - https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
wget https://packages.microsoft.com/config/debian/8/prod.list
sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list
```

[!INCLUDE [linux-apt-install-21](includes/linux-install-21-apt.md)]

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
wget https://packages.microsoft.com/config/debian/{os-version}/prod.list
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

使用包管理器进行安装时，将为你安装这些库。 但是，如果手动安装 .NET Core 或发布自包含的应用，则需要确保已安装以下库：

- libc6
- libgcc1
- libgssapi-krb5-2
- libicu52（适用于 8.x）
- libicu57（适用于9.x）
- libicu63（适用于10.x）
- libicu67（适用于11.x）
- libssl1.0.0（适用于8.x）
- libssl1.1（适用于9.x-11.x）
- libstdc++6
- zlib1g

对于使用 System.Drawing.Common 程序集的 .NET Core 应用，还需要以下依赖项：

- libgdiplus（版本 6.0.1 或更高版本）

  > [!WARNING]
  > 可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。 有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。

## <a name="scripted-install"></a>脚本安装

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手动安装

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>后续步骤

- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
