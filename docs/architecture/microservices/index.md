---
title: .NET 微服务。 适用于容器化 .NET 应用程序的体系结构
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 微服务可是模块化且可独立部署的服务。 （适用于 Linux 和 Windows）的 Docker 容器可将服务及其依赖项绑定到单个单元，使该单元在一个独立的环境中运行，因而可简化部署和测试。
ms.date: 11/10/2020
ms.openlocfilehash: 2055dacd46f90ba3714edb1437bcacad4c175e65
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94507262"
---
# <a name="net-microservices-architecture-for-containerized-net-applications"></a>.NET 微服务：适用于容器化 .NET 应用程序的体系结构

![封面](./media/cover-small.png)

**版本 v3.1** - 已更新到 ASP.NET Core v3.1

请参阅[更改记录](https://aka.ms/MicroservicesEbookChangelog)了解书籍更新和社区贡献。

本指南介绍如何使用容器开发基于微服务的应用程序并对其进行管理。 本指南探讨使用 .NET Core 和 Docker 容器的体系结构设计和实现方法。

为了更加轻松地开始使用，本指南重点介绍一个容器化和基于微服务的参考应用程序（用户可获取该应用程序）。 可通过 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) GitHub 存储库获取该参考应用程序。

## <a name="action-links"></a>操作链接

- 此电子书还提供 PDF 格式（仅限英文）[下载](https://aka.ms/microservicesebook)

- 克隆参考应用程序 [GitHub 上的 eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) 或为其创建分支

- 观看[第 9 频道上的介绍视频](https://aka.ms/microservices-video)

- 立即了解[微服务体系结构](https://aka.ms/MicroservicesArchitecture)

## <a name="introduction"></a>介绍

企业通过使用容器，日益实现成本节约、解决部署问题并改进 DevOps 和生产操作。 通过创建 Azure Kubernetes 服务、Azure Service Fabric 等产品，同时与 Docker、Mesosphere 和 Kubernetes 等行业领先者合作，Microsoft 一直在推出适用于 Windows 和 Linux 的容器创新。 这些产品提供容器解决方案，可帮助公司以云的速度和规模生成并部署应用程序，而无需考虑其选用的平台或工具。

Docker 正在逐渐成为容器行业的事实标准，受到 Windows 和 Linux 生态系统领域最重要供应商的支持。 （Microsoft 是支持 Docker 的主要云供应商之一。）将来，Docker 可能会在云端或本地的任何数据中心普及。

此外，[microservices](https://martinfowler.com/articles/microservices.html)（微服务）体系结构兴起，成为分布式任务关键型应用程序的重要方法。 在基于微服务的体系结构中，应用程序在可独立开发、测试、部署和版本控制的一系列服务上生成。

## <a name="about-this-guide"></a>关于本指南

本指南介绍如何使用容器开发基于微服务的应用程序并对其进行管理。 本指南探讨使用 .NET Core 和 Docker 容器的体系结构设计和实现方法。 为了更加轻松地开始使用容器和微服务，本指南重点介绍一个容器化和基于微服务的参考应用程序（用户可获取该应用程序）。 可通过 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) GitHub 存储库获取该示例应用程序。

本指南主要在开发环境级别提供基础开发和体系结构指导，重点介绍以下两种技术：Docker 和 .NET Core。 我们的目标是为用户在应用程序设计时提供指导，使用户无需将重点放在其生产环境的基础结构（云端或本地）上。 用户可在创建生产就绪的应用程序时，稍后制定有关基础结构的决策。 因此，本指南不区分基础结构，更侧重于考虑开发环境。

学习本指南后，接下来将了解 Microsoft Azure 上的生产就绪微服务。

## <a name="version"></a>Version

本指南已进行了修订，涵盖 .NET Core 3.1  版本以及与 .NET Core 3.1 同期的同一“批”技术（即 Azure 和其他第三方技术）的许多其他更新。 这就是书本版本也更新到 3.1 的原因  。

## <a name="what-this-guide-does-not-cover"></a>本指南未涵盖的内容

本指南未涵盖应用程序生命周期、DevOps、CI/CD 管道或团队协作。 补充指南 [Containerized Docker Application Lifecycle with Microsoft Platform and Tools](https://aka.ms/dockerlifecycleebook)（使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期）重点介绍该主题。 此外，当前指南不提供实现 Azure 基础结构的详细信息，例如有关特定业务流程的信息。

### <a name="additional-resources"></a>其他资源

- 《使用 Microsoft 平台和工具的容器化 Docker 应用程序生命周期》  （可下载电子书）  
    <https://aka.ms/dockerlifecycleebook>

## <a name="who-should-use-this-guide"></a>本指南的目标读者

本指南的目标读者是不熟悉以下内容的开发人员和解决方案架构师：基于 Docker 的应用程序开发和基于微服务的架构。 若要了解如何使用 Microsoft 开发技术（特别是 .NET Core）和 Docker 容器架构、设计和实现概念验证应用程序，本指南是理想之选。

如果企业架构师等技术决策制定者想要在综合了解体系结构和技术的基础上，作出有关新式和现代分布式应用程序应选择的方法的决策，则本指南也非常有用。

### <a name="how-to-use-this-guide"></a>如何使用本指南

本指南的第一部分介绍 Docker 容器，探讨用作开发框架时如何在 .NET Core 和 .NET Framework 之间选择，并提供有关微服务的概述。 此内容适合希望大概了解而不关注代码实现细节的架构师和技术决策制定者。

本指南的第二部分从[基于 Docker 的应用程序的开发过程](./docker-application-development-process/index.md)一节开始。 重点介绍使用 .NET Core 和 Docker 实现应用程序的开发和微服务模式。 对于要重点了解代码以及模式和实现详细信息的开发人员和架构师，此部分最有吸引力。

## <a name="related-microservice-and-container-based-reference-application-eshoponcontainers"></a>相关微服务和基于容器的参考应用程序：eShopOnContainers

eShopOnContainers 应用程序是用于 .NET Core 和旨在使用 Docker 容器部署的微服务的开源参考应用。 该应用程序包含多个子系统，包括多个网上商店 UI 前端（一个 Web MVC 应用、一个 Web SPA 和一个本机移动应用）。 还包括用于所有所需服务器端操作的后端微服务和容器。

应用程序的用途是展示体系结构模式。 **它不是可直接用于生产的模板**，无法启动实际应用程序。 事实上，应用程序处于永久 beta 状态，因为它还用来在出现有趣的新技术时对其进行测试。

## <a name="send-us-your-feedback"></a>向我们发送反馈！

本指南旨在帮助用户了解 .NET 中容器化应用程序和微服务的体系结构。 本指南和相关的参考应用程序会不断更新，欢迎你提供宝贵意见！ 如有关于本指南的改进建议，请在 <https://aka.ms/ebookfeedback> 提交反馈。

## <a name="credits"></a>信用

合著者：

> **Cesar de la Torre**，Microsoft Corp .NET 产品团队的高级项目经理。
>
> **Bill Wagner**，Microsoft Corp C+E 高级内容开发人员。
>
> **Mike Rousos**，Microsoft DevDiv CAT 团队的主要软件工程师

编辑：

> **Mike Pope**
>
> **Steve Hoag**

参与者和审阅者：

> **Jeffrey Ritcher**，Microsoft Azure 团队的合作伙伴软件工程师
>
> **Jimmy Bogard**，Headspring 的首席架构师
>
> **Udi Dahan**，Particular Software 的创始人兼 CEO
>
> **Jimmy Nilsson**，Factor10 的共同创始人兼 CEO
>
> **Glenn Condron**，ASP.NET 团队的高级项目经理
>
> **Mark Fussell**，Microsoft Azure Service Fabric 团队的主要项目经理主管
>
> **Diego Vega**，Microsoft 实体框架团队的项目经理主管
>
> **Barry Dorrans**，高级安全项目经理
>
> **Rowan Miller**，Microsoft 高级项目经理
>
> **Ankit Asthana**，Microsoft .NET 团队的主要项目经理
>
> **Scott Hunter**，Microsoft .NET 团队的合作伙伴总监项目经理
>
> Microsoft .NET 团队高级项目经理 Nish Anil 
>
> **Dylan Reisenberger**，Polly的架构师兼开发主管
>
> **Steve "ardalis" Smith** - 软件设计师及培训师 - [Ardalis.com](https://ardalis.com)
>
> **Ian Cooper**，Brighter 的编码架构师
>
> **Unai Zorrilla**，Plain Concepts 的架构师兼开发主管
>
> **Eduard Tomas**，Plain Concepts 的开发主管
>
> **Ramon Tomas**，Plain Concepts 的开发者
>
> **David Sanz**，Plain Concepts 的开发者
>
> **Javier Valero**，Grupo Solutio 的首席运营官
>
> **Pierre Millet**，Microsoft 高级顾问
>
> **Michael Friis**，Docker Inc 的产品经理
>
> **Charles Lowell**，Microsoft VS CAT 团队的软件工程师
>
> **Miguel Veloso**，Plain Concepts 的软件开发工程师
>
> Sumit Ghosh，Neudesic 的首席顾问

## <a name="copyright"></a>Copyright

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

>[!div class="step-by-step"]
>[下一页](container-docker-introduction/index.md)
