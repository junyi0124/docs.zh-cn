---
title: 在 RHEL 上安装 .NET - .NET
description: 演示在 RHEL 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 0b6138185bfd3e2f50c1b31e82779165715a5b6e
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851635"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-rhel"></a>在 RHEL 上安装 .NET SDK 或 .NET 运行时

RHEL 支持 .NET。 本文介绍如何在 RHEL 上安装 .NET。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="register-your-red-hat-subscription"></a>注册 Red Hat 订阅

若要在 RHEL 上从 Red Hat 安装 .NET，首先需要使用 Red Hat 订阅管理器进行注册。 如果未在系统上完成此操作，或者不确定是否完成了此操作，请参阅[适用于 .NET 的 Red Hat 产品文档](https://access.redhat.com/documentation/net/5.0/)。

## <a name="supported-distributions"></a>支持的发行版

下表列出了 RHEL 7 和 RHEL 8 上当前受支持的 .NET 版本。 这些版本在 [.NET 达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 RHEL 版本不再受到支持之前仍受支持。

- ✔️ 指示 RHEL 或 .NET 版本仍受支持。
- ❌ 指示 RHEL 或 .NET 版本在该 RHEL 版本上不受支持。
- 当 RHEL 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| RHEL                     | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](#rhel-8-)        | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [7](#rhel-7--net-50) | ✔️ 2.1        | ✔️ [3.1](#rhel-7--net-core-31)        | ✔️ [5.0](#rhel-7--net-50) |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="remove-preview-versions"></a>删除预览版本

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="how-to-install-other-versions"></a>如何安装其他版本

有关安装其他版本的 .NET 所需的步骤，请参阅[适用于 .NET 的 Red Hat 文档](https://access.redhat.com/documentation/net/5.0/)。

## <a name="rhel-8-"></a>RHEL 8 ✔️

.NET 包含在 RHEL 8 的 AppStream 存储库中。

[!INCLUDE [linux-dnf-install-50](includes/linux-install-50-dnf.md)]

## <a name="rhel-7--net-50"></a>RHEL 7 ✔️ .NET 5.0

以下命令安装 `scl-utils` 包：

```bash
sudo yum install scl-utils
```

### <a name="install-the-sdk"></a>安装 SDK

.NET SDK 使你可以通过 .NET 开发应用。 如果安装 .NET SDK，则无需安装相应的运行时。 若要安装 .NET SDK，请运行以下命令：

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet50 -y
scl enable rh-dotnet50 bash
```

Red Hat 建议不要永久启用 `rh-dotnet50`，因为这可能会影响其他程序。 如果要永久启用 `rh-dotnet`，请将以下行添加到 ~/.bashrc 文件中。

```bash
source scl_source enable rh-dotnet50
```

### <a name="install-the-runtime"></a>安装运行时

通过 .NET 运行时，可以运行使用 .NET 开发且未包含运行时的应用。 以下命令安装 ASP.NET Core 运行时，这是与 .NET Core 最兼容的运行时。 在终端中，运行以下命令。

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet50-aspnetcore-runtime-5.0 -y
scl enable rh-dotnet50 bash
```

Red Hat 建议不要永久启用 `rh-dotnet50`，因为这可能会影响其他程序。 如果要永久启用 `rh-dotnet50`，请将以下行添加到 ~/.bashrc 文件中。

```bash
source scl_source enable rh-dotnet50
```

作为 ASP.NET Core 运行时的一种替代方法，你可以安装不包含 ASP.NET Core 支持的 .NET 运行时：将上述命令中的 `rh-dotnet50-aspnetcore-runtime-5.0` 替换为 `rh-dotnet50-dotnet-runtime-5.0`。

## <a name="rhel-7--net-core-31"></a>RHEL 7 ✔️ .NET Core 3.1

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

以下命令安装 `scl-utils` 包：

```bash
sudo yum install scl-utils
```

### <a name="install-the-sdk"></a>安装 SDK

.NET Core SDK 使你可以通过 .NET Core 开发应用。 如果安装 .NET Core SDK，则无需安装相应的运行时。 若要安装 .NET Core SDK，请运行以下命令：

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31 -y
scl enable rh-dotnet31 bash
```

Red Hat 建议不要永久启用 `rh-dotnet31`，因为这可能会影响其他程序。 例如，`rh-dotnet31` 包括与基本 RHEL 版本不同的 `libcurl` 版本。 这可能会导致不需要不同版本的 `libcurl` 的程序出现问题。 如果要永久启用 `rh-dotnet`，请将以下行添加到 ~/.bashrc 文件中。

```bash
source scl_source enable rh-dotnet31
```

### <a name="install-the-runtime"></a>安装运行时

.NET Core 运行时允许运行使用不随附运行时的 .NET Core 所开发的应用。 以下命令安装 ASP.NET Core 运行时，这是与 .NET Core 最兼容的运行时。 在终端中，运行以下命令。

```bash
subscription-manager repos --enable=rhel-7-server-dotnet-rpms
yum install rh-dotnet31-aspnetcore-runtime-3.1 -y
scl enable rh-dotnet31 bash
```

Red Hat 建议不要永久启用 `rh-dotnet31`，因为这可能会影响其他程序。 例如，`rh-dotnet31` 包括与基本 RHEL 版本不同的 `libcurl` 版本。 这可能会导致不需要不同版本的 `libcurl` 的程序出现问题。 如果要永久启用 `rh-dotnet31`，请将以下行添加到 ~/.bashrc 文件中。

```bash
source scl_source enable rh-dotnet31
```

作为 ASP.NET Core 运行时的一种替代方法，你可以安装不包含 ASP.NET Core 支持的 .NET Core 运行时：将上述命令中的 `rh-dotnet31-aspnetcore-runtime-3.1` 替换为 `rh-dotnet31-dotnet-runtime-3.1`。

## <a name="snap"></a>Snap

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a>依赖项

[!INCLUDE [linux-rpm-install-dependencies](includes/linux-rpm-install-dependencies.md)]

## <a name="scripted-install"></a>脚本安装

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>手动安装

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>后续步骤

- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
