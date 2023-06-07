---
title: 监视容器化应用程序服务
description: 了解监控容器体系结构的一些关键方面
ms.date: 08/06/2020
ms.openlocfilehash: 5fcb5065d02018ab3c2d3ad71cf507bd43b53446
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87914948"
---
# <a name="monitor-containerized-application-services"></a>监视容器化应用程序服务

对于分成多个容器和微服务的应用程序来说，关键是要有一种方法来监视和分析整个应用程序的行为。

## <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://azure.microsoft.com/services/monitor/) 是一种可扩展的分析服务，可监视实时 Web 应用程序。 它可以帮助检测和诊断性能问题，并了解用户实际使用应用的情况。 它专为开发人员设计，旨在帮助你不断提高服务或应用程序的性能和可用性。 Azure Monitor 可与各种平台上的 Web/服务和独立应用程序配合使用，如 .NET、Java、Node.js 以及本地或云端托管的许多其他平台。

### <a name="additional-resources"></a>其他资源

- **Azure Monitor 概述** \
  <https://docs.microsoft.com/azure/azure-monitor/overview>

- **什么是 Application Insights？** \
  <https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview>

- **什么是 Azure Monitor 指标？** \
  <https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics>

- **Azure Monitor 中的容器监视解决方案** \
  <https://docs.microsoft.com/azure/azure-monitor/insights/containers>

## <a name="security-and-backup-services"></a>安全性和备份服务

为了确保应用程序和基础设施处于最佳状态以支持业务需求，必须处理许多细节方面的支持工作，而在微服务领域，情况变得更加复杂，因此需要一种在需要采取行动时既有高级视图又有详细视图的方法。

Azure 具有管理和提供云和本地资源的四个关键方面的统一视图的工具：

- **安全性**。 使用 [Azure 安全中心](https://azure.microsoft.com/services/security-center/)。
  - 全面了解并控制虚拟机、应用和工作负载的安全性。
  - 集中管理安全策略并集成现有流程和工具。
  - 使用高级分析检测真实威胁。

- **备份**。 使用 [Azure 备份](https://azure.microsoft.com/services/backup/)。
  - 避免成本高昂的业务中断，满足符合性目标，并保护数据免受勒索软件和人为错误的影响。
  - 保持备份数据在传输和静止时加密。
  - 确保基于多因素身份验证的访问，以防止未经授权的使用。

- **本地资源**。 使用[混合云解决方案](https://azure.microsoft.com/solutions/hybrid-cloud-app/)。

>[!div class="step-by-step"]
>[上一页](manage-production-docker-environments.md)
>[下一页](../key-takeaways/index.md)
