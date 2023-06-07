---
title: 使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序
description: 本指南提供了使用 ASP.NET Core 和 Azure 生成单片式 Web 应用的端到端指导。
author: ardalis
ms.author: wiwagn
ms.date: 12/07/2020
ms.openlocfilehash: 90d092b2269315e5b6734430e82cc7211324479b
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851290"
---
# <a name="architect-modern-web-applications-with-aspnet-core-and-azure"></a>使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序

![《架构新式 Web 应用程序》指南的书籍封面图像。](./media/index/web-application-guide-cover-image.png)

**EDITION v5.0** - 已更新到 ASP.NET Core 5.0

请参阅[更改记录](https://aka.ms/aspnet-ebook-changelog)了解书籍更新和社区贡献。

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 © 2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

Docker 的鲸鱼徽标是 Docker Inc. 的注册商标经许可方可使用。

所有其他标记和徽标均为其各自所有者的财产。

作者:

> **Steve "ardalis" Smith** - 软件设计师及培训师 - [Ardalis.com](https://ardalis.com)

编辑：

> Maira Wenzel

## <a name="action-links"></a>操作链接

- 此电子书还提供 PDF 格式（仅限英文）[下载](https://aka.ms/webappebook)

- 克隆参考应用程序 [GitHub 上的 eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb) 或为其创建分支

## <a name="introduction"></a>介绍

相比传统 .NET 开发，.NET 5 和 ASP.NET Core 具有一系列优势。 如果下列所有或部分方面对于你的应用程序成功至关重要，那么应对服务器应用程序使用 .NET 5：

- 跨平台支持。

- 微服务的使用。

- Docker 容器的使用。

- 高性能和可伸缩性需求。

- 在同一服务器上通过应用程序对 .NET 版本进行并行版本控制。

传统 .NET 应用程序虽然支持上述多项要求，但是 ASP.NET Core 和 .NET 5 已经过优化，可为以上方案提供完善的支持。

越来越多的组织选择使用 Microsoft Azure 等服务，在云中托管 web 应用程序。 如果以下方面对你的应用程序或组织至关重要，应该考虑在云中托管应用程序：

- 减少对数据中心的成本投入（硬件、软件、空间、实用工具、服务器管理等）

- 灵活的定价（基于使用情况付费，无需为空闲容量付费）。

- 高可靠性。

- 改善应用移动性；可轻松更改部署应用的位置和方式。

- 灵活的容量；基于实际需要增加或减少。

使用 Azure 中托管的 ASP.NET Core 生成 Web 应用，与传统替代方法相比，这能提供许多竞争优势。 ASP.NET Core 针对新式 web 应用程序开发做法和云托管方案进行了优化。 本指南介绍如何构建 ASP.NET Core 应用程序以充分利用这些功能。

## <a name="version"></a>Version

本指南已经过修订，现涵盖 .NET 5.0 版本，还包含与.NET 5.0 同期的同一“批”技术（即 Azure 和其他第三方技术）的诸多其他更新。 这就是书本版本也更新到 5.0 的原因。

## <a name="purpose"></a>目标

本指南提供了使用 ASP.NET Core 和 Azure 构建单片 Web 应用程序的端到端指导。 在此上下文中，“单片”是指这一事实，即这些应用程序会作为单个单元部署，而不是作为交互服务和应用程序的集合。

本指南是 [“ _.NET 微服务 - 容器化 .NET 应用程序体系结构_”](../microservices/index.md)的补充，该文章更侧重于介绍 Docker、微服务和部署容器以托管企业应用程序。

### <a name="net-microservices-architecture-for-containerized-net-applications"></a>.NET 微服务。 适用于容器化 .NET 应用程序的体系结构

- 电子书  
  <https://aka.ms/MicroservicesEbook>
- 示例应用程序  
  <https://aka.ms/microservicesarchitecture>

## <a name="who-should-use-this-guide"></a>本指南的目标读者

本指南的受众主要是对使用云中的 Microsoft 技术和服务生成新式 web 应用程序感兴趣的开发者、开发潜在顾客和架构师。

次要受众是技术决策者，他们已经熟悉 ASP.NET 或 Azure，并想要了解是否需要为新项目或现有项目升级到 ASP.NET Core。

## <a name="how-you-can-use-this-guide"></a>如何使用本指南

本指南已精简为较小的文档，侧重介绍如何使用新式 .NET 技术和 Azure 生成 Web 应用程序。 可通读本指南，了解有关此类应用程序及其技术注意事项的基本信息。 本指南及其示例应用程序还可作为操作起点或参考。 可将相关示例应用程序作为你自己的应用程序的模板，或者了解如何组织应用程序的组件部件。 在对自己的应用程序进行选择权衡时，请参考指南的原则、体系结构的范围以及技术选项和决策注意事项。

请将本指南转发到团队中，这有助于确保对这些注意事项和机会的共同理解。 确保每个人使用共同的术语和基础原则工作，这有助于构建模式和做法的一致应用。

## <a name="references"></a>reference

- **为服务器应用选择 .NET 5 或 .NET Framework**  
  [https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server](../../standard/choosing-core-framework-server.md)

>[!div class="step-by-step"]
>[下一页](modern-web-applications-characteristics.md)
