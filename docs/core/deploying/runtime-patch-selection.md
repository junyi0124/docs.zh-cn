---
title: .NET Core 自包含应用部署的运行时前滚。
description: 了解自包含部署的 dotnet publish 更改。
author: KathleenDollard
ms.date: 05/31/2018
ms.openlocfilehash: 22385c7b5d2bf87755fd51cd6268d21fe3431c74
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740794"
---
# <a name="self-contained-deployment-runtime-roll-forward"></a>自包含部署运行时前滚

.NET Core [自包含应用程序部署](index.md)包括 .NET Core 库和 .NET Core 运行时。 从 .NET Core 2.1 SDK（版本 2.1.300）开始，自包含应用程序部署[在计算机上发布最高版本的修补程序运行时](https://github.com/dotnet/designs/pull/36)。 默认情况下，自包含部署的 [`dotnet publish`](../tools/dotnet-publish.md) 选择作为发布计算机上 SDK 一部分而安装的最新版本。 这让部署的应用程序在 `publish` 期间能与安全修补程序（以及其他修补程序）配合运行。 若要获取新的修补程序，需要重新发布应用程序。 自包含应用程序是通过在 `dotnet publish` 命令上指定 `-r <RID>` 创建的，或是通过在项目文件 (csproj / vbproj) 或命令行中指定[运行时标识符 (RID)](../rid-catalog.md) 创建的。

## <a name="patch-version-roll-forward-overview"></a>修补程序版本前滚概述

[`restore`](../tools/dotnet-restore.md)、[`build`](../tools/dotnet-build.md) 和 [`publish`](../tools/dotnet-publish.md) 是可以单独运行的 `dotnet` 命令。 运行时选择属于 `restore` 操作，而不是 `publish` 或 `build` 操作。 如果调用 `publish`，则会选择最新的修补程序版本。 如果调用带有 `--no-restore` 参数的 `publish`，则可能不会获取所需的修补程序版本，因为没有先使用新的自包含应用程序发布策略执行 `restore`。 在这种情况下，会出现生成错误，并显示以下类似文本：

  “已使用 Microsoft.NETCore.App 版本 2.0.0 还原该项目，但是按照当前设置，将改为使用版本 2.0.6。 若要解决此问题，请确保将相同的设置用于 restore 和后续操作，例如 build 或 publish。 如果在 build 或 publish 期间设置了 RuntimeIdentifier 属性，而没有在 restore 过程中设置，通常就会出现此问题。”

> [!NOTE]
> `restore` 和 `build` 可以作为另一个命令（例如 `publish`）的组成部分隐式运行。 当作为另一个命令的组成部分隐式运行时，它们会带有附加的上下文，以便生成正确的项目。 如果使用某个运行时（例如 `dotnet publish -r linux-x64`）执行 `publish`，隐式的 `restore` 会还原 linux-x64 运行时的包。 如果显示调用 `restore`，则默认情况下它不会还原运行时包，因为没有上下文。

## <a name="how-to-avoid-restore-during-publish"></a>如何避免在 publish 过程中 restore

你可能在进行 `publish` 操作时不需要运行 `restore`。 在创建自包含应用程序时，若要避免在 `publish` 过程中进行 `restore`，请执行以下操作：

- 将 `RuntimeIdentifiers` 属性设为一个分号分隔的列表，其中包含所有要发布的 [RID](../rid-catalog.md)。
- 将 `TargetLatestRuntimePatch` 属性设置为 `true`。

## <a name="no-restore-argument-with-dotnet-publish-options"></a>使用 dotnet publish 选项的 no-restore 参数

如果要使用同样的项目文件创建自包含应用程序和[依赖框架的应用程序](index.md)，并且想通过 `dotnet publish` 使用 `--no-restore` 参数，请选择以下各项之一：

1. 首选依赖框架的行为。 如果是依赖框架的应用程序，则此选项为默认行为。 如果是自包含应用程序，并且能使用未带修补程序的 2.1.0 本地运行时，请在项目文件中将 `TargetLatestRuntimePatch` 设为 `false`。

2. 首选自包含行为。 如果是自包含应用程序，则此选项为默认行为。 如果是依赖框架的应用程序，且需要安装最新版本的修补程序，请在项目文件中将 `TargetLatestRuntimePatch` 设为 `true`。

3. 通过在项目文件中将 `RuntimeFrameworkVersion` 设为特定的修补程序版本，可对运行时框架版本进行显式控制。
