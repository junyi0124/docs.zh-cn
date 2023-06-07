---
title: Docker 应用程序的外部循环 DevOps 工作流步骤
description: 使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期
ms.date: 08/06/2020
ms.openlocfilehash: 1a973407d59484899f99fb6e326b8d7c8e97079b
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87915220"
---
# <a name="creating-cicd-pipelines-in-azure-devops-services-for-a-net-core-application-on-containers-and-deploying-to-a-kubernetes-cluster"></a>在 Azure DevOps Services 中为容器中的 .NET Core 应用程序创建 CI/CD 管道并将其部署到 Kubernetes 群集

在图 5-12 中，可以看到端到端 DevOps 场景，包括代码管理、代码编译、Docker 映像生成、Docker 映像推送到 Docker 注册表并最后部署到 Azure 中的 Kubernetes 群集。

![工作流程：在开发计算机中启动。 通过推送到存储库，开始使用推送到 Docker 注册表的自定义映像生成/CI 任务，然后用于 CD/部署任务，最终将其推送到 AKS。](media/docker-workflow-ci-cd-aks.png)

**图 5-12**。 创建 Docker 映像并将其部署到 Azure 中的 Kubernetes 群集的 CI/CD 场景

请务必注意通过 Docker 注册表（如Docker Hub 或 Azure 容器注册表）连接的生成/CI 和发布/CD 这两个管道。 与不带 Docker 的传统 CI/CD 进程相比，Docker 注册表是主要区别之一。

如图 5-13 所示，第一阶段是生成/CI 管道。 在 Azure DevOps Services 中，可以创建生成/CI 管道，这些管道将编译代码、创建 Docker 映像，并将其推送到 Docker Hub 或 Azure 容器注册表等 Docker 注册表。

![Azure DevOps 生成流程任务定义的浏览器视图。](media/build-ci-pipeline-azure-devops-push-to-docker-registry.png)

**图 5-13**。 Azure DevOps 中的生成 Docker 映像并将映像推送到 Docker 注册表的生成/CI 管道

第二个阶段是创建部署/发布管道。 在 Azure DevOps Services 中，可以使用 Azure DevOps Services 的 Kubernetes 任务轻松创建面向 Kubernetes 群集的部署管道（如图 5-14 所示）。

![Azure DevOps 部署到 Kubernetes 任务定义的浏览器视图。](media/release-cd-pipeline-azure-devops-deploy-to-kubernetes.png)

**图 5-14**。 Azure DevOps 服务中部署到 Kubernetes 群集的发布/CD 管道

> [!Walkthrough] 将 eShopModernized 部署到 Kubernetes：
>
> 如需了解部署到 Kubernetes 的 Azure DevOps CI/CD 管道的详细演练，请参阅以下帖子：\
><https://github.com/dotnet-architecture/eShopModernizing/wiki/04.-How-to-deploy-your-Windows-Containers-based-apps-into-Kubernetes-in-Azure-Container-Service-(Including-CI-CD)>

>[!div class="step-by-step"]
>[上一页](docker-application-outer-loop-devops-workflow.md)
>[下一页](../run-manage-monitor-docker-environments/index.md)
