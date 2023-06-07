---
title: 无服务器应用：体系结构、模式和 Azure 实现
description: 无服务器体系结构指南。 了解何时、为何以及如何为企业应用程序实现无服务器体系结构（相对于基础结构即服务 [IaaS] 或平台即服务 [PaaS]）。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 04/22/2020
ms.openlocfilehash: 867765d29a7c50694a5de7b1de56346d86600a83
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171814"
---
# <a name="serverless-apps-architecture-patterns-and-azure-implementation"></a>无服务器应用：体系结构、模式和 Azure 实现

![显示无服务器应用电子书封面的屏幕截图。](./media/index/serverless-apps-cover-v3.png)

版本 v3.0 - 已更新到 Azure Functions v3 

> 下载地址：<https://aka.ms/serverlessbookpdf>

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 &copy; 2018-2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

所有其他标记和徽标均为其各自所有者的财产。

作者:

> [Jeremy Likness](https://twitter.com/jeremylikness)，Microsoft Corp. 的 .NET 数据高级项目经理 

参与者：

> Microsoft Corp. 高级云大使 **[Cecil Phillip](https://twitter.com/cecilphillip)**

编辑：

> Microsoft Corp. 高级内容开发者 **[Bill Wagner](https://twitter.com/billwagner)**

> Microsoft Corp. 高级内容开发者 **[Maira Wenzel](https://twitter.com/mairacw)**

参与者和审阅者：

> [Steve Smith](https://twitter.com/ardalis)，Ardalis Services 所有者  。

## <a name="introduction"></a>介绍

[无服务器](https://azure.microsoft.com/solutions/serverless/)是云平台向纯云本机代码方向的进化。 通过无服务器，开发者更接近业务逻辑，避免基础结构问题。 这种模式并不意味着“没有服务器”，而是“更少的服务器”。 无服务器代码是事件驱动的。 无论是传统的 HTTP Web 请求，还是计时器或上传文件的结果都可以触发代码。 无服务器背后的基础结构可实现即时缩放以满足弹性需求，并提供微计费以真正“为所使用的量付费”。 无服务器需要一种新的生成应用程序的思维和方法，它并不是解决每个问题的正确解决方案。 作为开发者，必须确定：

- 无服务器的优点和缺点是什么？
- 为什么要考虑为自己的应用程序使用无服务器？
- 如何生成、测试、部署和维护无服务器代码？
- 在现有应用程序中将代码迁移到无服务器有什么意义？完成此转换的最佳方法是什么？

## <a name="about-this-guide"></a>关于本指南

本指南重点介绍使用无服务器的应用程序的云本机开发。 本书重点介绍了这些优点并揭示了开发无服务器应用的潜在缺点，并提供了对无服务器体系结构的调查。 介绍了有关使用无服务器的许多示例以及各种无服务器设计模式。

本指南介绍了 Azure 无服务器平台的组件，并重点介绍了如何使用 [Azure Functions](/azure/azure-functions/functions-overview) 实现无服务器。 你将了解触发器和绑定以及如何使用 Durable Functions 实现依赖于状态的无服务器应用。 最后，业务示例和案例研究将有助于提供用于确定无服务器是否是项目正确方法的上下文和参考框架。

## <a name="evolution-of-cloud-platforms"></a>云平台的演变

无服务器是云平台多次迭代的巅峰。 这种演变始于数据中心的物理计算机，然后经历了基础结构即服务 (IaaS) 和平台即服务 (PaaS) 的发展阶段。

![从本地到无服务器的演变](./media/serverless-evolution-iaas-paas.png)

在云之前，开发和运营之间存在明显的界限。 部署应用程序需要回答无数问题，例如：

- 应该安装什么硬件？
- 如何保护对计算机的物理访问？
- 数据中心是否需要不间断电源 (UPS)？
- 存储备份发送到何处？
- 应该有冗余电源吗？

此类问题不胜枚举，相关开销巨大。 在许多情况下，IT 部门不得不处理大量浪费。 浪费是由服务器过度分配导致的，例如用于灾难恢复的备份计算机以及用于横向扩展的备用服务器。幸运的是，随着虚拟化技术（如 [Hyper-V](/virtualization/hyper-v-on-windows/about/)）和虚拟机 (VM) 的引入，产生了基础结构即服务 (IaaS)。 虚拟化基础结构允许运营将一组标准服务器设置为主干网，从而形成一个灵活的环境，能够“按需”预配唯一服务器。 更重要的是，虚拟化为使用云提供虚拟机“即服务”奠定了基础。 公司不必担心冗余电源或物理计算机的问题。 他们可专注于虚拟环境。

IaaS 仍然需要大量开销，因为运营仍然负责执行各种任务。 这些任务包括：

- 修补和备份服务器。
- 安装包。
- 使操作系统保持最新状态。
- 监控应用程序。

新的发展通过平台即服务 (PaaS) 减少了开销。 借助 PaaS，云提供程序可处理操作系统、安全修补程序和支持特定平台所需的包。 开发人员只需选择平台目标（例如“Web 应用程序”或“API 终结点”）并直接部署代码，而不用生成 VM，然后配置 .NET 并启用 Internet Information Services (IIS) 服务器。 基础结构问题减少到：

- 需要什么大小的服务？
- 如何横向扩展服务（添加更多服务器或节点）？
- 如何纵向扩展服务（增加托管服务器或节点的容量）？

无服务器通过专注于事件驱动代码进一步抽象化服务器。 开发者可关注执行一项操作的微服务，而非平台。 生成无服务器代码的两个关键问题是：

- 什么触发代码？
- 代码会执行哪些操作？

通过无服务器，抽象化基础结构。 在某些情况下，开发者不再担心主机。 无论是运行 IIS、Kestrel、Apache 还是其他 Web 服务器的实例来管理 Web 请求，开发者都会关注 HTTP 触发器。 触发器为请求提供标准的跨平台有效负载。 有效负载不仅简化了开发过程，而且便于测试，在某些情况下，使代码可跨平台轻松移植。

无服务器的另一个功能是微计费。 Web 应用通常托管 Web API 终结点。 在传统的裸机、IaaS 和 PaaS 实现中，托管 API 的资源是连续付费的。 这意味着即使没有访问终结点，也需要付费来托管终结点。 通常会发现调用某个 API 比调用其他 API 的次数更多，因此整个系统基于支持热门终结点进行缩放。 借助无服务器，可独立缩放每个终结点并支付使用费用，因此在不调用 API 时不会产生任何成本。 在许多情况下，迁移可以大大降低用于支持终结点的持续成本。

## <a name="what-this-guide-doesnt-cover"></a>本指南未涵盖的内容

本指南特别强调了体系结构方法和设计模式，并未深入探讨 Azure Functions、[逻辑应用](/azure/logic-apps/logic-apps-what-are-logic-apps)或其他无服务器平台的实现细节。 本指南不包括逻辑应用的高级工作流或 Azure Functions 的功能等，如配置跨源资源共享(CORS)、应用自定义域或上传 SSL 证书。 可以通过联机 [Azure Functions 文档](/azure/azure-functions/functions-reference)获得这些详细信息。

### <a name="additional-resources"></a>其他资源

- [Azure 体系结构中心](/azure/architecture/)
- [云应用程序的最佳做法](/azure/architecture/best-practices/api-design)

## <a name="who-should-use-the-guide"></a>本指南的目标读者

本指南面向具有以下需求的开发者和解决方案架构师：需要使用 .NET 生成企业应用程序（这些应用程序可能使用本地或云端的无服务器组件）。 可为具有以下需求的开发者、架构师和技术决策制定者提供帮助：

- 了解无服务器开发的优点和缺点
- 学习如何处理无服务器体系结构
- 无服务器应用实现的示例

## <a name="how-to-use-the-guide"></a>如何使用本指南

本指南的第一部分通过比较几种不同的体系结构方法来探讨无服务器的可行性。 本指南分析了技术和开发生命周期，因为软件开发的所有方面都受到体系结构决策的影响。 然后，本指南介绍了用例和设计模式，并说明了使用 Azure Functions 的参考实现。 每个部分都包含介绍特定方面的其他资源。 该指南最后提供了有关无服务器实现的演练和实践探索的资源。

## <a name="send-your-feedback"></a>提供你的反馈

我们正在不断完善指南和相关示例，欢迎你提供反馈！ 如果对如何改进本指南有任何建议，请使用 [GitHub 问题](https://github.com/dotnet/docs/issues)上任何页面底部的反馈部分进行反馈。

>[!div class="step-by-step"]
>[下一页](architecture-approaches.md)
