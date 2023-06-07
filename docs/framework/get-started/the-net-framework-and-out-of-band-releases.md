---
title: .NET Framework 和带外版本
description: 了解 .NET 和带外版本。 发布了一些带外 (OOB) 的新功能，可改进跨平台开发或引入新的功能。
ms.date: 10/10/2018
ms.assetid: 721f10fa-3189-4124-a00d-56ddabd889b3
ms.openlocfilehash: 9653696f46279e0c23418f92030d64839cc20518
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618761"
---
# <a name="net-framework-and-out-of-band-releases"></a>.NET Framework 和带外版本

.NET Framework 不断发展，可适应不同的平台（例如 UWP 应用、传统桌面和 Web 应用）并最大限度地实现代码重用。 除了定期的 .NET Framework 发布之外，还会发布一些带外 (OOB) 的新功能，改进跨平台开发或引入新的功能。

## <a name="advantages-of-oob-releases"></a>OOB 版本的优点

通过将新组件或更新发送给带外组件，使 Microsoft 能够更频繁地提供 .NET Framework 的更新。 此外，我们可以更快地收集和回应客户反馈。

如果你在应用中使用了 OOB 功能，你的用户不必安装最新版 .NET Framework 即可运行你的应用，因为 OOB 程序集是与你的应用包一起部署的。

## <a name="how-oob-packages-are-distributed"></a>OOB 包是如何存储的

核心公共语言运行时 (CLR) 组件的 OOB 版本通过 [NuGet](https://www.nuget.org/)（.NET 的包管理器）提供。 通过 NuGet，你可轻松地从 Visual Studio 内浏览库并将其添加至你的 .NET Framework 项目。 从 Visual Studio 2012 开始，NuGet 包管理器随附于 Visual Studio 的所有版本。 在 Visual Studio 的“工具菜单”中，找到“NuGet 包管理器” 。 如果未安装，请按照[安装 NuGet](/nuget/install-nuget-client-tools) 上的说明进行操作。 有关 NuGet 的详细信息，请参阅 [NuGet 文档](/nuget)。

## <a name="use-a-nuget-oob-package"></a>使用 NuGet OOB 包

如果 NuGet 包管理器已安装，你可使用 Visual Studio 中的解决方案资源管理器浏览并添加对 NuGet 包的引用：

1. 在 Visual Studio 中打开项目的快捷菜单，然后选择“管理 NuGet 包”。 （也可以在“项目”菜单中选择此选项。）

2. 在左侧窗格中，选择“联机”。

3. 如果要使用预发行包，请选择中间窗格内下拉列表框中的“包括预发行版”（而不选择“仅限稳定版”）。

4. 在右侧窗格中，使用“搜索”框来查找要使用的包。 某些 Microsoft 程序包通过 Microsoft .NET Framework 徽标进行识别，发布者均为 Microsoft。

![NuGet 包管理器。](./media/the-net-framework-and-out-of-band-releases/nuget-package-manager-dialog.png)

如前所述，当你部署使用了 OOB 包的应用程序，OOB 程序集将随附你的应用程序包。

## <a name="types-of-oob-releases"></a>OOB 版本的类型

通常情况下，OOB 程序包具有一个或多个预发行版本和一个稳定的版本。 预发行附带的许可证通常不允许再发行，但可让你试用程序包并提供反馈。 任何对包的更新中都包含反馈。 最终发布版本作为稳定的包随 NuGet 分发并包含允许随应用程序重新分发 NuGet 程序包的许可证。 稳定程序包受 Microsoft 支持。 Microsoft 还提供 IntelliSense 支持以及其他文档类型（如所有程序包的博客文章和论坛答案）。 此外，只有一些程序包可使用源代码，并非所有程序包都可使用。 有关新增和更新后的包的公告，可订阅 [.NET Framework 博客](https://devblogs.microsoft.com/dotnet/)。

若要查找预发行版包和稳定版包，请在 NuGet 包管理器中选择“包括预发行版”。

## <a name="see-also"></a>请参阅

- [入门](index.md)
