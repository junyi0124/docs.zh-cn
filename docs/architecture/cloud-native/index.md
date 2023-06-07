---
title: 为 Azure 构建云本机 .NET 应用程序
description: 构建利用 Azure 的容器、微服务和无服务器功能的云本机应用程序的指南。
author: ardalis
ms.date: 11/10/2020
ms.openlocfilehash: 673bfef27c3767f68b1c30d4383cee010ba377f0
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506644"
---
# <a name="architecting-cloud-native-net-applications-for-azure"></a>为 Azure 构建云本机 .NET 应用程序

![封面图像](./media/cover.png)

**版本 v1.0**

请参阅[更改记录](https://aka.ms/cn-ebook-changelog)了解书籍更新和社区贡献。

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

作者：

> Rob Vettor，Microsoft 首席云系统架构师/IP 架构师 ([thinkingincloudnative.com](https://thinkingincloudnative.com/about/))
>
> Steve "ardalis" Smith，[Ardalis.com](https://ardalis.com) 软件设计师及培训师

参与者和审阅者：

> **Cesar De Torre**，Microsoft .NET 团队首席项目经理
>
> Nish Anil，Microsoft .NET 团队高级项目经理
>
> Jeremy Likness，Microsoft .NET 团队高级项目经理
>
> Cecil Phillip，Microsoft 高级云大使

编辑：

> Maira Wenzel，Microsoft .NET 团队高级项目经理

## <a name="version"></a>Version

本指南的编写涵盖 .NET Core 3.1 版本以及与 .NET Core 3.1 同期的同一“批”技术（即 Azure 和其他第三方技术）的许多其他更新。

## <a name="who-should-use-this-guide"></a>本指南的目标读者

本指南的受众主要是对学习如何构建为云设计的应用程序感兴趣的开发人员、开发主管和架构师。

次级受众是计划选择是否使用云本机方法构建其应用程序的技术决策者。

## <a name="how-you-can-use-this-guide"></a>如何使用本指南

本指南首先定义云本机，并介绍使用云本机原则和技术构建的参考应用程序。 除了前两章，本书内容分成特定章节，集中讨论大多数云本机应用程序的共通主题。 可以跳转到以下任何章节，了解相关云本机方法：

- 数据和数据访问
- 通信模式
- 缩放和可伸缩性
- 应用程序复原能力
- 监视和运行状况
- 标识和安全性
- DevOps

本指南有 [PDF](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf) 格式和在线版本。 欢迎随时将此文档或其在线版本的链接转发给你的团队，以确保对这些主题有共同的理解。 这些主题中的大多数都得益于对基本原则和模式的一致理解，以及与这些主题相关的决策所涉及的权衡。 本文档的目标是为团队及其领导提供所需的信息，使其能够为应用程序的体系结构、开发和托管作出明智的决策。

## <a name="send-your-feedback"></a>提供你的反馈

我们正在不断完善本书和相关示例，欢迎你提供反馈！ 如果对如何改进本书有任何建议，请使用 [GitHub 问题](https://github.com/dotnet/docs/issues)上任何页面底部的反馈部分进行反馈。

>[!div class="step-by-step"]
>[下一页](introduction.md)
