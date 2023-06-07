---
title: 通过云中的 CI/CD 管道和 DevOps 工具保持对应用的生命周期进行现代化
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 通过云中的 CI/CD 管道和 DevOps 工具保持现代化应用的生命周期
ms.date: 04/30/2018
ms.openlocfilehash: 98ebd29b8ab81c8fff6da546942825133f06f4de
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172048"
---
# <a name="modernize-your-apps-lifecycle-with-cicd-pipelines-and-devops-tools-in-the-cloud"></a>通过云中的 CI/CD 管道和 DevOps 工具保持对应用的生命周期进行现代化

如今的企业需要快速创新，才能在市场中获得竞争力。 交付高质量的新式应用程序需要 DevOps 工具和进程，这些工具和进程对于实现这一不断创新的循环至关重要。 借助适当的 DevOps 工具，开发人员可以简化持续部署，并更快地让用户获得创新的应用程序。

尽管持续集成和部署做法已建立良好的发展，但引入容器也会引入新的注意事项，尤其是在使用多容器应用程序时。

Azure DevOps Services 通过官方 Azure DevOps Services 部署任务支持将多容器应用程序持续集成和部署到各种环境：

- [部署到用于容器的 Azure Web 应用](/azure/devops/pipelines/apps/cd/deploy-docker-webapp?tabs=dotnet-core)

- [部署到 Azure Kubernetes 服务](/azure/devops/pipelines/apps/cd/deploy-aks?tabs=dotnet-core)

不过，你还可以使用基于 Azure DevOps Services 脚本的任务部署到 [Docker Swarm](https://blog.jcorioland.io/archives/2016/11/29/full-ci-cd-pipeline-to-deploy-multi-containers-application-on-azure-container-service-docker-swarm-using-visual-studio-team-services.html) 或 DC/OS。

为了继续促进部署灵活性，这些工具为容器工作负载提供了开发到测试到生产部署的完美体验，同时提供了一系列开发和 CI/CD 解决方案。

图 4-12 显示了部署到 Azure 容器服务中的 Kubernetes 群集的持续部署管道。

![Azure DevOps 服务部署到 Kubernetes 群集的屏幕截图。](./media/life-cycle-ci-cd-pipelines-devops-tools/deploy-mvc-app-container-kubernetes.png)

**图 4-12.** Azure DevOps Services 持续部署管道，部署到 Kubernetes 群集

>[!div class="step-by-step"]
>[上一页](modernize-your-apps-with-monitoring-and-telemetry.md)
>[下一页](migrate-to-hybrid-cloud-scenarios.md)
