---
title: 云本机的候选应用
description: 了解哪些类型的应用程序受益于云本机方法
author: robvet
ms.date: 05/14/2020
ms.openlocfilehash: 8f00633a575dad12b0bc1d5adb83acac03db0659
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160939"
---
# <a name="candidate-apps-for-cloud-native"></a>云本机的候选应用

查看公文包中的应用。 有多少用户有资格使用云本机体系结构？ 它们全部？ 也许有些吗？

应用成本/收益分析后，就很有可能不支持巨大价格标记，这是云本机所必需的。 云本机的成本远远超出了应用程序的商业价值。

什么类型的应用程序可能是云本机的候选项？

- 需要不断发展业务能力/功能的战略企业系统

- 需要高发布速度的应用程序，具有高置信度

- 一种系统，其中的各个功能必须在不完全重新部署整个系统的情况 *下* 发布

- 团队开发的应用程序，具有不同技术领域的专业知识

- 具有必须独立缩放的组件的应用程序

然后有一些旧系统。 尽管我们都喜欢构建新的应用程序，但我们经常负责现代化对企业至关重要的旧工作负荷。 随着时间的推移，旧应用程序可以分解为微服务，并最终将 "replatformed" 分解为云本机体系结构。

### <a name="modernizing-legacy-apps"></a>现代化旧版应用

免费的 Microsoft 电子书 [通过 Azure 云和 Windows 容器实现现有 .net 应用程序的现代化，](https://dotnet.microsoft.com/download/thank-you/modernizing-existing-net-apps-ebook) 为将本地工作负荷迁移到云中提供了指导。 图1-10 显示，没有适用于现代化旧版应用程序的单个大小合适的策略。

![迁移旧工作负荷的策略](./media/strategies-for-migrating-legacy-workloads.png)

**图 1-10**。 迁移旧工作负荷的策略

不关键的单一应用在很大程度上得益于快速迁移 ([云基础结构](../modernize-with-azure-containers/lift-and-shift-existing-apps-azure-iaas.md) 的) 迁移。 此处，本地工作负荷重新承载基于云的 VM，无需更改。 此方法使用 [IaaS (基础结构即服务) 模型](https://azure.microsoft.com/overview/what-is-iaas/)。 Azure 包含多个工具，如 [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)、 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/)和 [Azure 数据库迁移服务](https://azure.microsoft.com/campaigns/database-migration/) ，使此类移动更容易。 尽管此策略可能会节省成本，但这种应用程序通常不会构建为解锁和利用云计算的好处。

对业务至关重要的单一应用程序通常会受益于改进的 (*云优化*) 迁移。 此方法包括启用关键云服务的部署优化，无需更改应用程序的核心体系结构。 例如，你可能会 [容器化](/virtualization/windowscontainers/about/) 该应用程序并将其部署到容器 orchestrator，如 [Azure Kubernetes Services](https://azure.microsoft.com/services/kubernetes-service/)，如本书稍后部分所述。 一旦进入云中，应用程序就可以使用其他云服务，例如数据库、消息队列、监视和分布式缓存。

最后，执行战略性企业功能的单一应用程序可能最大程度地受益于 *云本机* 方法（本书的主题）。 此方法可提供灵活性和速度。 但这种情况下，它需要 replatforming、重新架构和重写代码。

如果你和你的团队相信云本机方法合适，则可以 behooves 你的组织制定决策。 云本机方法将解决什么问题？ 它如何与业务需求保持一致？

- 更自信地快速发布功能？

- 精细的可伸缩性-更有效地使用资源？

- 提高了系统复原能力？

- 提高了系统性能？

- 更深入地了解操作？

- 融合开发平台和数据存储以获得适用于该作业的最佳工具？

- 未来的应用程序投资证明？

适当的迁移策略取决于组织的优先级和目标系统。 对于许多人来说，对单一应用程序进行云优化或将粗粒度服务添加到 N 层应用程序可能更具成本效益。 在这些情况下，你仍可以充分利用 cloud PaaS 功能，如 Azure App Service 提供的功能。

## <a name="summary"></a>总结

本章介绍了云本机计算。 我们提供了一个定义，同时提供了驱动云本机应用程序的关键功能。 我们探讨了可证明这种投资和工作量的应用程序类型。

随着简介，我们现在深入了解云本机。

### <a name="references"></a>参考

- [云本机计算基础](https://www.cncf.io/)

- [.NET 微服务：容器化 .NET 应用程序的体系结构](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [利用 Azure 云和 Windows 容器实现现有 .NET 应用程序的现代化](https://dotnet.microsoft.com/download/thank-you/modernizing-existing-net-apps-ebook)

- [云本机模式（按 Cornelia Davis）](https://www.manning.com/books/cloud-native-patterns)

- [超过十二个因素的应用程序](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)

- [什么是基础结构即代码](/azure/devops/learn/what-is-infrastructure-as-code)

- [Uber 工程的微部署：自信地部署](https://eng.uber.com/micro-deploy/)

- [Netflix 如何部署代码](https://www.infoq.com/news/2013/06/netflix/)

- [缩放 WeChat 微服务的重载控制](https://www.cs.columbia.edu/~ruigu/papers/socc18-final100.pdf)

>[!div class="step-by-step"]
>[上一页](definition.md)
>[下一页](introduce-eshoponcontainers-reference-app.md)
