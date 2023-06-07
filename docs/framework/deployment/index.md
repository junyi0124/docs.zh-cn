---
title: 部署 .NET Framework 和应用程序
description: 开始随应用程序部署 .NET。 .NET 提供不受影响的应用程序、默认情况下的专用组件和可控的代码共享等功能。
ms.date: 03/30/2017
helpviewer_keywords:
- deploying applications [.NET Framework], packaging
- deploying applications [.NET Framework]
- deploying applications [.NET Framework], features
- deploying applications [.NET Framework], distribution
- .NET Framework, deploying
- .NET Framework application deployment
ms.assetid: 238d8284-6042-4a38-a7f6-1ee8efd719da
ms.openlocfilehash: 9948d5313c5168965f3ff991b26a4bc913f7d7ee
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817024"
---
# <a name="deploying-the-net-framework-and-applications"></a>部署 .NET Framework 和应用程序

本文有助于你开始部署应用程序的 .NET Framework。 大部分信息都是供开发人员、OEM 和企业管理人员使用的。 想在其自己的计算机上安装 .NET Framework 的用户应阅读[安装 .NET Framework](../install/index.md)。

## <a name="key-deployment-resources"></a>关键部署资源

使用以下指向启用 MSDN 主题的链接了解关于部署和维护 .NET Framework 的特定信息。

**安装和部署**

- 安装程序和部署的一般信息：

  - 安装程序选项：

    - [Web 安装程序](../install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable)

    - [脱机安装程序](../install/guide-for-developers.md#to-install-or-download-the-net-framework-redistributable)

  - 安装模式：

    - [无提示安装](deployment-guide-for-developers.md#chaining_custom)

    - [显示 UI](deployment-guide-for-developers.md#chaining_default)

  - [在 .NET Framework 4.5 安装期间减少系统重新启动次数](reducing-system-restarts.md)

  - [安装和卸载 .NET Framework 受阻疑难解答](../install/troubleshoot-blocked-installations-and-uninstallations.md)

- 部署客户端应用程序中的 .NET Framework（适用于开发人员）：

  - 在安装和部署项目中[使用 InstallShield](deployment-guide-for-developers.md#installshield-deployment)

  - [使用 Visual Studio ClickOnce 应用程序](deployment-guide-for-developers.md#clickonce-deployment)

  - [创建 WiX 安装包](deployment-guide-for-developers.md#wix)

  - [使用自定义安装程序](deployment-guide-for-developers.md#chaining)

  - 适用于开发人员的[其他信息](deployment-guide-for-developers.md)

- 部署.NET Framework（适用于 OEM 和管理人员）：

  - [Windows 评估和部署工具包 (ADK)](/windows-hardware/get-started/adk-install)

  - [管理员指南](guide-for-administrators.md)

**服务**

- 有关一般信息，请参阅 [.NET Framework 博客](https://devblogs.microsoft.com/dotnet/)。

- [检测版本](../migration-guide/how-to-determine-which-versions-are-installed.md)

- [检测服务包和更新](../migration-guide/how-to-determine-which-net-framework-updates-are-installed.md)

## <a name="features-that-simplify-deployment"></a>简化部署的功能

.NET Framework 提供了大量基本功能，让部署应用程序变得更加容易：

- 不产生影响的应用程序。

     此功能提供应用程序隔离功能并消除 DLL 冲突。 默认情况下，组件不会影响其他应用程序。

- 默认情况下的私有组件。

     默认情况下，组件部署到应用程序目录，并且仅对包含的应用程序可见。

- 可控的代码共享。

     代码共享要求你显式提供代码以便进行共享而不是执行默认行为。

- 并行版本。

     组件或应用程序的多个版本可以共存，你可以选择要使用的版本类型，公共语言运行时可以增强版本策略。

- XCOPY 部署和复制。

     在没有注册表项或依赖项的情况下就可以部署自描述和自持有的组件和应用程序。

- 动态更新。

     管理员可以使用宿主（例如，ASP.NET）更新程序 Dll，即使在远程计算机也可以。

- 与 Windows 安装程序相集成。

     部署应用程序时，播发、发布、修复和按需安装都可用。

- 企业部署。

     此功能提供了简单的软件分发，包括使用 Active Directory。

- 下载和缓存。

     增量下载可使下载大小变得更小，并且组件可以相互隔离，使其只供部署影响较低的应用程序使用。

- 部分受信任代码。

     标识是基于代码而不是基于用户，并且不会出现任何证书对话框。

## <a name="packaging-and-distributing-net-framework-applications"></a>打包和分发 .NET Framework 应用程序

文档中的其他章节介绍了打包和部署 .NET Framework 的部分信息。 这些章节介绍了以下关于自描述单元的信息：[程序集](../../standard/assembly/index.md)（不需要注册表项）、[强名称程序集](../../standard/assembly/strong-named.md)（确保名称的唯一性和避免名称欺骗）和[程序集版本控制](../../standard/assembly/versioning.md)（可解决很多与 DLL 冲突相关的问题）。 以下各节提供有关打包和分发.NET Framework 应用程序的信息。

### <a name="packaging"></a>打包

.NET Framework 提供了用于打包应用程序的以下选项：

- 作为单个程序集或作为程序集的集合。

     使用此选项，你只需使用创建的 .dll 或 .exe 文件即可。

- 作为 cabinet (CAB) 文件。

     使用此选项，将文件压缩成 .cab 文件可使分发或下载耗时更少。

- 为 Windows Installer 程序包或其他安装程序格式。

     使用此选项，你可以创建用于 Windows Installer 的 .msi 文件或打包用于其他安装程序的应用程序。

### <a name="distribution"></a>分布

.NET Framework 提供了用于分发应用程序的以下选项：

- 使用 XCOPY 或 FTP。

     由于公共语言运行时应用程序是自描述的并且不需要注册表项，因此，你可以使用 XCOPY 或 FTP 只将应用程序复制到相应的目录。 然后即可从该目录运行该应用程序。

- 使用代码下载。

     如果你要通过 Internet 或公司的 Intranet 分发应用程序，只需将代码下载到计算机并在下载位置运行应用程序即可。

- 使用安装程序（例如 Windows Installer 2.0）。

     Windows Installer 2.0 可以安装、修复或删除全局程序集缓存和专用目录中的 .NET Framework 程序集。

### <a name="installation-location"></a>安装位置

要确定部署应用程序的程序集的位置，以便通过运行时找到它们，请参阅[运行时如何定位程序集](how-the-runtime-locates-assemblies.md)。

安全注意事项也可能会影响你部署应用程序的方式。 根据代码所在的位置授予托管代码安全权限。 将应用程序或组件部署到得不到信任的位置（例如 Internet），可限制应用程序或组件可以执行的操作。 有关部署和安全注意事项的详细信息，请参阅[代码访问安全性基础知识](../misc/code-access-security-basics.md)。

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----------|-----------------|
|[运行时如何定位程序集](how-the-runtime-locates-assemblies.md)|描述公共语言运行时如何确定将用于满足绑定请求的程序集类型。|
|[适用于程序集加载的最佳做法](best-practices-for-assembly-loading.md)|讨论避免类型标识问题的方法，从而避免发生 <xref:System.InvalidCastException><xref:System.MissingMethodException> 和其他错误。|
|[在 .NET Framework 4.5 安装期间减少系统重新启动次数](reducing-system-restarts.md)|描述在任何可能的情况下可以阻止重启的 Restart Manager，并说明安装 .NET Framework 的应用程序如何利用它。|
|[面向管理员的部署指南](guide-for-administrators.md)|说明系统管理员如何通过 Microsoft Endpoint Configuration Manager 跨网络部署 .NET Framework 及其系统依赖项。|
|[面向开发人员的部署指南](deployment-guide-for-developers.md)|说明开发人员如何在其计算机的应用程序中安装 .NET Framework。|
|[部署应用程序、服务和组件](/visualstudio/deployment/deploying-applications-services-and-components)|讨论 Visual Studio 中的部署选项，包括使用 ClickOnce 和 Windows Installer 技术发布应用程序的说明。|
|[发布 ClickOnce 应用程序](/visualstudio/deployment/publishing-clickonce-applications)|描述打包 Windows 窗体应用程序以及使用 ClickOnce 将其部署到连网的客户端计算机上的方法。|
|[打包和部署资源](../resources/packaging-and-deploying-resources-in-desktop-apps.md)|描述 .NET Framework 用于打包和部署资源的中心辐射模型，包括资源命名约定、后备进程和打包替代项。|
|[部署互操作应用程序](../interop/deploying-an-interop-application.md)|说明如何传送和安装互操作应用程序，通常包括 .NET Framework 客户端程序集、表示区分 COM 类型库的一个或多个互操作程序集以及一个或多个已注册的 COM 组件。|
|[如何：获取 .NET Framework 4.5 安装程序的进度](how-to-get-progress-from-the-dotnet-installer.md)|描述如何在没有提示的情况下启动和跟踪 .NET Framework 安装进度，同时显示你自己的安装进度视图。|

## <a name="see-also"></a>请参阅

- [开发指南](../development-guide.md)
