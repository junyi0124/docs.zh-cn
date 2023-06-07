---
title: 使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期
description: 在基本层面概要了解使用 Docker 和 Microsoft 平台及工具开发和部署容器化应用程序的过程。
ms.date: 11/10/2020
ms.openlocfilehash: cf20ea97ec252649cdb14add40ead67b6319520a
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506657"
---
# <a name="containerized-docker-application-lifecycle-with-microsoft-platform-and-tools"></a>使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期

![封面](./media/devops-book-cover-large-we.png)

**版本 v3.1** - 已更新到 ASP.NET Core v3.1

请参阅[更改记录](https://aka.ms/DockerLifecycleEbookChangelog)了解书籍更新和社区贡献。

本指南在整体上概述了使用 Microsoft 平台和工具通过 Docker 开发和部署容器化的 ASP.NET Core 应用程序。 本指南大致介绍了 Azure DevOps（用于实现 CI/CD 管道）以及 Azure 容器注册表 (ACR) 和 Azure Kubernetes 服务 AKS（用于部署）。

关于开发的大致详细信息，可参阅 [.NET 微服务：适用于容器化 .NET 应用程序的体系结构](../microservices/index.md) 及其相关的参考应用程序 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)。

## <a name="send-us-your-feedback"></a>向我们发送反馈！

本指南旨在帮助用户了解 .NET 中容器化应用程序和微服务的体系结构。 本指南和相关的参考应用程序会不断更新，欢迎你提供宝贵意见！ 如有关于本指南的改进建议，请在 <https://aka.ms/ebookfeedback> 提交反馈。

## <a name="credits"></a>信用

作者:

> **Cesar de la Torre**，Microsoft Corp .NET 产品团队的高级项目经理。

策划编辑：

> Janine Patrick

开发编辑：

> Bob Russell，Microsoft 的解决方案专业人员
>
> [Octal Publishing, Inc.](http://www.octalpub.com/)

生产编辑：

> [Dianne Russell](http://www.octalpub.com/)
>
> Octal Publishing, Inc.

文字加工编辑：

> Bob Russell，Microsoft 的解决方案专业人员

参与者和审阅者：

> Microsoft .NET 团队高级项目经理 Nish Anil 
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

版权所有 &copy; 2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

Docker 的鲸鱼徽标是 Docker Inc. 的注册商标经许可方可使用。

所有其他标记和徽标均为其各自所有者的财产。

>[!div class="step-by-step"]
>[下一页](introduction-to-containers-and-docker.md)
