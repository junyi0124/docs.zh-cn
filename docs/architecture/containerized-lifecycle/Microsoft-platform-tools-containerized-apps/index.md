---
title: 有关适用于容器化应用的 Microsoft 平台和工具的简介
description: 了解 Microsoft 提供来支持 Docker 应用程序生命周期的产品/服务。
ms.date: 08/06/2020
ms.openlocfilehash: 72b98f945494936b7775a525297a17c5ce72c69a
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916075"
---
# <a name="introduction-to-the-microsoft-platform-andtools-for-containerized-apps"></a>适用于容器化应用的 Microsoft 平台和工具的简介

*愿景：创建一个适应性强且已容器化的企业级应用程序生命周期，它横跨开发、IT 操作和生产管理。*

图 3-1 显示 Docker 应用的生命周期的主要支柱，这些应用根据多个团队（应用开发、DevOps 基础结构流程和 IT 管理和运营）交付的工作类型分类。 通常，在企业中，负责每个方面的“角色”的个人资料是不同的。 其技能也是如此。

![Visual Studio 中的“添加新项目”窗口，已选择 ASP.NET Core Web 应用。](media/index/microsoft-tools-contanerized-docker-app.png)

**图 3-1。** 使用 Microsoft 平台和工具的容器化 Docker 应用程序的生命周期中的主要支柱

容器化的 Docker 生命周期工作流最初可根据“默认产品选择”设定规范，以便开发人员更轻松地更快入门，但从根本上来说，必须具有一个开放框架，使其成为可适应每个组织或企业中不同上下文的灵活工作流。 工作流基础结构（组件和产品）必须足够灵活，覆盖每个公司未来会拥有的环境，甚至能够与其他人交换开发或 DevOps 产品。 正如以下章节所述，平台和基础结构的灵活性、开放性和广泛的技术选择正是 Microsoft 对于容器化 Docker 应用程序最重视的几个方面。

表 3-1 显示了适用于容器化 Docker 应用程序的 Azure DevOps 的目的，即提供开放式 DevOps 工作流，使用户可选择要用于每个阶段（Microsoft 或第三方）的产品，同时提供具有已互联的“默认产品”的简化工作流，从而使用户可快速使用适用于 Docker 应用的企业级 DevOps 工作流。

**表 3-1.** 向所有技术公开的 Azure DevOps 工作流

| 主机 | Microsoft 技术 | 第三方（Azure 可插入） |
| ---------------------------| ----------------------------------------------------| --------------------------------------------------------------------------------|
| Docker 应用平台   | • Microsoft Visual Studio 和 Visual Studio Code<br /> • .NET<br /> • Microsoft Azure Kubernetes 服务 (AKS)<br /> • Azure 容器注册表<br /> | •任何代码编辑器（如 Sublime）<br /> •任何语言（Node.js、Java、 Go 等）<br /> •任何业务流程协调程序和计划程序<br />  •任何 Docker 注册表<br /> |
| Docker 应用的 DevOps     | • Azure DevOps Services<br /> • Microsoft Team Foundation Server<br /> • Azure Kubernetes 服务 (AKS)<br /> | • GitHub、Git、Subversion 等<br /> • Jenkins、Chef、Puppet、Velocity、CircleCI、TravisCI 等<br /> • 本地 Docker 数据中心、Kubernetes、Mesos DC/OS 等<br /> |
| 管理与监视  | • Azure Monitor | • Marathon、 Chronos 等<br />|

如表 3-1 所定义，适用于容器化 Docker 应用的 Microsoft 平台和工具包含以下组件：

- **适用于 Docker 应用开发的平台** 构建“应用”的一个服务或服务集合的开发。 开发平台提供将代码推送到共享代码存储库前，开发人员所需的所有工作。 开发部署为容器的服务与开发不使用 Docker 的相同应用或服务很相似。 你可继续使用首选语言（.NET、Node.js、Go 等）和首选编辑器/IDE（如 Visual Studio 或 Visual Studio Code）。 但是，请不要将 Docker 视为部署目标，你是在 Docker 环境中开发服务。 可以在本地生成、运行、测试和调试容器中的代码，以在开发时提供目标环境。 通过本地提供目标环境，Docker 容器可通过设置大幅度改善 DevOps 生命周期。 Visual Studio 和 Visual Studio Code 具有可在开发过程中集成 Docker 容器的扩展。

- **适用于 Docker 应用的 DevOps** 创建 Docker 应用程序的开发人员可使用 [Azure DevOps](https://azure.microsoft.com/services/devops/) 或任何其他第三方产品（如 Jenkins）来构建全方位自动化应用程序生命周期管理 (ALM)。

  借助 Azure DevOps，开发人员可创建专注于容器的 DevOps 来实现快速迭代过程，该过程涵盖任意位置（Azure DevOps-Git、GitHub、任何远程 Git 存储库或 Subversion）的源代码管理、持续集成 (CI)、内部单元测试、内部容器/服务集成测试、持续交付 (CD) 和发布管理 (RM)。 从开发到暂存和生产环境，开发人员还能将其 Docker 应用程序版本自动处理到 Azure Kubernetes Service (AKS) 中。

- **管理和监视** IT 人员可按多种方式管理和监视生产应用程序和服务，将这两方面集成到一个整合后的体验中。

  - **Azure 门户** Azure Kubernetes 服务 (AKS) 可帮助你设置和维护 Docker 环境。 你还可使用其他业务流程协调程序来直观显示和配置群集。

  - **Docker 工具**  可以使用熟悉的工具管理容器应用程序。 无需更改现有 Docker 管理做法，即可将容器工作负载移动到云。 针对所选的业务流程协调程序，使用熟悉的应用程序管理工具并通过标准 API 终结点连接。 你还可使用其他第三方工具管理 Docker 应用程序，甚至可使用 CLI Docker 工具进行管理。

    即使你已熟悉 Linux 命令，也可通过 Linux Subsystem 命令行和在此 Linux Subsystem 功能上运行的产品（Docker 和 Kubernetes 等）客户端，使用 Microsoft Windows 和 PowerShell 管理容器应用程序。 你将在本书稍后部分深入学习如何通过你喜欢的 Microsoft Windows OS 在 Linux Subsystem 下使用这些工具。

  - **开源工具**  由于 AKS 公开了业务流程引擎的标准 API 终结点，因此 AKS 与热门工具兼容，而且在大多数情况下，这些工具是现成可用的，其中包括可视化工具、监视、命令行工具和要在未来推出的工具（可用时）。

  - **Azure Monitor** 它是 Azure 的解决方案，可监视生产环境的各个方面。 你只需将其 SDK 设置到服务中，以便能从应用程序中获取系统生成的日志数据，即可监视生产 Docker 应用程序。

因此，Microsoft 为端到端容器化 Docker 应用程序生命周期提供了完整的基础。 但是，它汇集了产品和技术，让你能够根据需要选择并与现有工具和流程相集成  。 凭借大量方法的灵活性和功能深度优势，Microsoft 在容器化 Docker 应用程序开发领域占据有力地位。

>[!div class="step-by-step"]
>[上一页](../Docker-application-lifecycle/containers-foundation-for-devops-collaboration.md)
>[下一页](../design-develop-containerized-apps/index.md)
