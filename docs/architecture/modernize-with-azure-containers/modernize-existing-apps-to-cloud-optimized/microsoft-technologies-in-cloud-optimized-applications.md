---
title: 云优化应用程序中的 Microsoft 技术
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 云优化应用程序中的 Microsoft 技术
ms.date: 04/28/2018
ms.openlocfilehash: b257497835638dd65c894998e95bd7e9d784b7bf
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172009"
---
# <a name="microsoft-technologies-in-cloud-optimized-applications"></a>云优化应用程序中的 Microsoft 技术

以下列表描述了可识别为云优化应用程序要求的工具、技术和解决方案。 你可以根据自己的优先级有选择地或逐步地采用云优化元素。

- **云基础结构**：提供计算平台、操作系统、网络和存储的基础结构。 Microsoft Azure 位于此级别。

- **运行时**：此层提供应用程序的运行环境。 如果使用容器，则此层通常基于在 Linux 主机或 Windows 主机上运行的 [Docker 引擎](https://docs.docker.com/engine/)。 （从 Windows Server 2016 开始，支持 [Windows 容器](/virtualization/windowscontainers/about/)。 Windows 容器是在 Windows 上运行的现有 .NET Framework 应用程序的最佳选择。）

- **托管云**：选择托管云选项后，你可以避免在管理和支持底层基础结构、VM、OS 修补程序和网络配置方面的开销和复杂性。 如果选择使用 IaaS 进行迁移，则需要负责所有这些任务以及相关的成本。 在托管云选项中，只管理你开发的应用程序和服务。 云服务提供商通常会管理其他所有内容。 Azure 中托管云服务的示例包括 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database)、[Azure Redis 缓存](https://azure.microsoft.com/services/cache/)、[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)、[Azure 存储](https://azure.microsoft.com/services/storage/)、[Azure Database for MySQL](https://azure.microsoft.com/services/mysql/)、[Azure Database for PostgreSQL](https://azure.microsoft.com/services/postgresql/)、[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 和托管计算服务（如 [VM 规模集](https://azure.microsoft.com/services/virtual-machine-scale-sets/)）、[Azure 应用服务](https://azure.microsoft.com/services/app-service/)以及 [Azure Kubernetes 服务](https://azure.microsoft.com/services/container-service/)。

- **应用程序开发**：生成在容器中运行的应用程序时，可以选择多种语言。 本指南重点介绍 [.NET](https://dotnet.microsoft.com)，但你可以使用其他语言（如 node.js、Python、Spring/Java 或 Go）来开发基于容器的应用。

- **监视、遥测、日志记录和审核**：对于任何云优化应用程序，监视和审核云中运行的应用程序和容器的能力至关重要。 [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 和 [Microsoft Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) 是为云优化应用提供监视和审核的主要 Microsoft 工具。

- **预配**：自动化工具可帮助你预配基础结构和将应用程序部署到多个环境（生产、测试、暂存）。 可以使用 Chef 和 Puppet 之类的工具来管理应用程序的配置和环境。 此层还可以通过使用更简单、更直接的方法来实现。 例如，你可以使用 Azure 命令行接口 (Azure CLI) 工具直接部署，然后在 [Azure DevOps Services](https://azure.microsoft.com/services/devops/) 中使用连续部署和发布管理管道。

- **应用程序生命周期**：[Azure DevOps Services](https://azure.microsoft.com/services/devops/) 和其他工具（如 Jenkins）都是构建自动化服务器，可帮助你实现 CI/CD 管道，包括发布管理。

本章的后续部分以及相关演练专门介绍有关运行时层（Windows 容器）的详细信息。 本指南描述在 Windows Server 2016 （及更高版本）VM 和 Azure 容器实例上部署 Windows 容器的方式。 它还介绍了更高级的 PaaS 平台（如 Azure 应用服务）和业务流程协调程序（如 Azure Kubernetes 服务）。

## <a name="monolithic-applications-can-be-cloud-optimized"></a>整体式应用程序可以  是云优化的应用程序

强调整体式应用程序（不基于微服务的应用程序）可以  为云优化的应用程序是非常重要的。 你可以使用容器、持续交付和 DevOps 的组合来构建和运行利用云计算模型的整体式应用程序。 如果现有的整体式应用程序适用于你的业务目标，则可以将其现代化，并使其成为云优化应用程序。

同样，如果整体式应用程序可以是云优化应用程序，其他更复杂的体系结构（如 N 层应用程序）也可以现代化为云优化应用程序。

>[!div class="step-by-step"]
>[上一页](reasons-to-modernize-existing-net-apps-to-cloud-optimized-applications.md)
>[下一页](what-about-cloud-native-applications.md)
