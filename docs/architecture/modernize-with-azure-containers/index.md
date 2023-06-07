---
title: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序（第 2 版）
description: 阅读此电子书，了解如何将现有应用程序直接迁移到 Azure 云和 Windows 容器以实现现代化。
ms.date: 04/28/2018
ms.openlocfilehash: f4ae4e2d24d343b55811955fb43e929c0db6f01b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705327"
---
# <a name="modernize-existing-net-applications-with-azure-cloud-and-windows-containers-2nd-edition"></a>通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序（第 2 版）

![现代化 .NET 应用程序指南的封面图像。](./media/index/web-application-guide-cover-image.png)

由 Microsoft 出版社和 Microsoft Corporation 的 Microsoft DevDiv 部门出版，华盛顿州雷蒙德市 One Microsoft Way 98052-6399

版权所有 © 2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制本书中的任何内容。

本书以电子书 (e-book) 形式免费提供，可通过 Microsoft 的多个通道获得，如 <https://dot.net/architecture>。

若要了解本书的相关问题，请发送电子邮件至 [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com?subject=Feedback%20for%20.NET%20Container%20&%20Microservices%20Architecture%20book)。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。 所有其他标记均为其各自所有者的财产。

作者:
> **Cesar de la Torre**，Microsoft Corp .NET 产品团队的高级项目经理。

参与者和审阅者：
> .NET 团队的合作伙伴总监项目经理 Scott Hunter、Microsoft Visual Studio Tools 团队的主要项目经理 Paul Yuknewicz、Microsoft Visual Studio Tools 团队的高级项目经理 Lisa Guthrie、Microsoft .NET 团队的主要项目经理 Ankit Asthana、Microsoft Plain Concepts 的开发者领导 Unai Zorrilla、Grupo Solutio 的首席运营官 Javier Valero

## <a name="introduction"></a>介绍

决定现代化 Web 应用程序或服务并将其迁移到云时，无需完全重新构建应用。 由于成本和时间限制，使用微服务之类的高级方法来重新构建应用程序并非始终可行。 根据应用程序的类型，也可能无需重新构建应用。 为了优化组织云迁移策略的成本效益，必须考虑业务和应用的需求。 需要确定：

- 哪些应用需要转换或重新构建。

- 哪些应用只需部分更新。

- 哪些应用能直接“直接迁移”到云。

## <a name="about-this-guide"></a>关于本指南

本指南主要重点介绍现有 Microsoft .NET Framework Web 应用程序或面向服务的应用程序的初始现代化，即将工作负荷移动到更新或更现代化的环境，而不会显著改变应用程序的代码和基本体系结构。

本指南还强调使用一组特定的新技术和方法（如，Windows 容器和 Azure 中支持 Windows 容器的相关计算平台）将应用移动到云并部分现代化应用的益处。

## <a name="path-to-the-cloud-for-existing-net-applications"></a>将现有 .NET 应用程序迁移到云的途径

组织通常选择移动到云来使其应用程序获得敏捷性和速度。 只需几分钟即可在云中设置数千台服务器 (VM)，而设置本地服务器通常需要数周。

将应用程序迁移到云没有普遍通用的策略。 合适的迁移方法由组织的需求和优先考虑事项，以及要迁移的应用程序种类决定。 并非所有应用程序都保证可以移动到平台即服务 ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)) 模型或开发[云原生](https://www.gartner.com/doc/3181919/architect-design-cloudnative-applications)应用程序模型。 许多情况下都可根据业务需求采用阶段性或递增性方法将资产移动到云。

对于可为组织带来最佳长期敏捷性和价值的现代化应用程序，投资云本机应用程序体系结构是有益的。  但是，对于现有或旧的应用程序资产，关键在于用最少的时间和金钱将应用程序移动到云（无需重新构建或更改代码），从而实现显著的效益。

图 1-1 显示了采用递增性方法将现有 .NET 应用程序移动到云时可能使用的途径。

 ![现有 .NET 应用程序和服务的更新途径](./media/image1-1.png)

**图 1-1**。 现有 .NET 应用程序和服务的更新途径

每种迁移方法的优点和使用原因各不相同。 可以选择一种方法将应用迁移到云，也可以从多种方法中选择某些组件。 单个应用程序不限于单一的方法或成熟度状态。 例如，常见的混合方法将包含某些本地组件以及云中的其他组件。

每个应用程序成熟度的定义和简短说明如下所示：

**级别 1：云基础结构就绪** 的应用程序：使用此迁移方法，将当前本地应用程序迁移或重新托管到基础结构即服务 ([IaaS](https://azure.microsoft.com/overview/what-is-iaas/)) 平台即可。 应用的组成与之前基本一致，但现在可将它们部署到云中的 VM。
这种简单类型的迁移在业内通常称为“直接迁移”。

**级别 2：云优化** 的应用程序：在此级别，仍无需重新架构或改写大量代码，可以利用容器和其他云托管服务等现代化技术在云中运行应用，从而获得更多益处。 通过优化企业开发操作 (DevOps) 流程，可以提高应用程序的敏捷性，以实现更快交付。 这可以通过使用某种技术（如基于 Docker 引擎的 Windows 容器）实现。 分多个阶段部署时，容器可以消除因应用程序依赖项造成的冲突。 在此成熟度模型中，可以在 IaaS 或 PaaS 上部署容器，同时使用与数据库、缓存即服务、监视以及持续集成/持续部署 (CI/CD) 管道相关的其他云托管服务。

第三个成熟度级别是云中的最终目标，但对于很多应用它是可选的，并不是本指南的主要重点：

**级别 3：云本机** 应用程序：此迁移方法通常由业务需求驱动，目的是更新任务关键型应用程序。 在此级别，使用 PaaS 服务将应用移动到 PaaS 计算平台。 实现[云原生](https://www.gartner.com/doc/3181919/architect-design-cloudnative-applications)应用程序和微服务体系结构，以改进应用程序，使其具有长期的敏捷性并扩展至新的限制。 此类更新通常需要专门针对云进行构建。 通常必须编写新代码，尤其是移动到基于云原生应用程序和微服务的模型时。 此方法有助于获得一些益处，这些益处在单片和本地应用程序环境中很难实现。

表 1-1 介绍了选择每种迁移或更新方法的主要优点和原因。

| **云基础结构就绪** <br /> *直接迁移* | **云优化** <br /> *现代化* | **云本机** <br /> *现代化、重新架构和重写* |
|---|---|---|
| **应用程序的计算目标** |
| 部署到 Azure 中 VM 的应用程序 | 部署到 Azure 应用服务、Azure 容器实例 (ACI)、包含容器的 VM 或 AKS（Azure Kubernetes 服务）的单层或 N 层应用 | Azure Kubernetes 服务 (AKS) 上的容器化的微服务和/或基于 Azure Functions 的无服务器微服务。 |
| **数据目标** |
| VM 中的 SQL 或任何关系数据库 | Azure SQL 数据库托管实例或云中的其他托管数据库。 | 针对每项微服务的精细数据库，基于 Azure SQL 数据库、Azure Cosmos DB 或云中其他托管数据库 |
| **优点**|
| <li>无需重新架构，无需编写新代码 <li> 需要的工作量最少，实现快速迁移 <li> Azure 中支持最小公分母 <li> 保证基本可用性 <li> 移动到云后，更容易更新 | <li> 无需重新架构 <li> 极少代码/配置更改 <li> 由于容器的原因，改进了部署和 DevOps 发布敏捷性 <li> 增加了密度，降低了部署成本 <li> 应用的可移植性和依赖项 <li> 主机目标的灵活性：PaaS 方法或 IaaS | <li> 针对云进行架构，可以从云获得最大优势，但需要新代码 <li> 微服务云原生方法 <li> 现代化任务关键型应用程序，具有云复原能力，可实现超大规模缩放 <li> 完全托管服务 <li> 优化了规模 <li> 通过子系统优化了自主敏捷性 <li> 基于部署和 DevOps |
| **挑战** |
| <li> 除了转移运营费用或关闭数据中心之外，云价值较小 <li> 几乎无需管理：无 OS 或中间件修补；可使用基础架构解决方案，如 Terraform、Spinnaker 或 Puppet | <li> 容器化是开发者和 IT 运营学习曲线中一个额外的步骤 <li> DevOps 和 CI/CD 管道通常是此方法的“必备项”。 如果组织文化中当前没有这些必备品，这可能是一个额外的挑战| <li> 进行现代化时，需要重新架构云本机应用和微服务体系结构，并且通常需要进行大量的代码重构或重写（增加时间和预算）|
> **表 1-1**。 现有 .NET 应用程序和服务的更新途径的优势和挑战

### <a name="key-technologies-and-architectures-by-maturity-level"></a>各成熟度级别使用的关键技术和体系结构

.NET Framework 应用程序的初始版本为 .NET Framework 1.0，该版本于 2001 年年底发布。 然后，公司发布了更新的版本（如 2.0、3.5 和 .NET Framework 4.x）。 这些应用程序中的大部分都在 Windows Server 和 Internet Information Server (IIS) 上运行，并使用关系数据库，如 SQL Server、Oracle、MySQL 或任何其他关系数据库管理系统。

当今大部分现有 .NET 应用程序可能都基于 .NET Framework 4.x，甚至 .NET Framework 3.5，并使用 Web 框架，如 ASP.NET MVC、ASP.NET Web 窗体、ASP.NET Web API、Windows Communication Foundation (WCF)、ASP.NET SignalR 和 ASP.NET 网页。 这些既定的 .NET Framework 技术都依赖于 Windows。 如果只是简单地迁移旧版应用并希望对应用程序基础结构作出最少的更改，依赖项是一项重要的考虑因素。

图 1-2 显示了三个云成熟度级别中每一个级别所使用的主要技术和体系结构样式：

![用于更新现有 .NET Web 应用程序的每个成熟度级别的主要技术](./media/image1-2.png)

**图 1-2**。 用于更新现有 .NET Web 应用程序的每个成熟度级别的主要技术

图 1-2 强调了最常见的方案，但在体系结构方面，可能存在许多混合变体。 例如，成熟度模型不仅适用于现有 Web 应用的单片体系结构，还适用于服务方向、N 层以及其他体系结构样式变体。 一种或另一种体系结构类型及相关技术的关注度或占比更高，决定了应用程序的总体成熟度。

更新流程中的每个成熟度级别与以下关键技术和方法相关：

- **云基础结构就绪**（重新托管或基础直接迁移）：作为第一步，很多组织只想快速执行云迁移策略。 在这种情况下，要重新托管应用程序。 大多数重新托管均可使用 [Azure Migrate](https://aka.ms/azuremigrate) 自动完成，该服务提供指南、见解和机制，帮助迁移到基于 Azure 的云工具（如 [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) 和 [Azure 数据库迁移服务](https://azure.microsoft.com/campaigns/database-migration/)）。 也可以手动设置重新托管，以便在将旧版应用移动到云时了解关于这些资产的基础结构详细信息。 例如，可以在只做些许更改（或许只对配置做出细微更改）的情况下将应用程序移动到 Azure 中的 VM。 这种情况下的网络类似于本地环境，尤其是在 Azure 中创建虚拟网络时。

- **云优化**（托管服务和 Windows 容器）：此模型进行了一些重要的部署优化，可获得云带来的显著益处，并且不需要更改应用程序的核心体系结构。 此处的基本步骤是对现有 .NET Framework 应用程序增加 [Windows 容器](/virtualization/windowscontainers/about/) 支持。 此重要步骤（容器化）不需要触及代码，因此直接迁移的总体工作量很少。 可以使用 [Image2Docker](https://github.com/docker/communitytools-image2docker-win) 或 Visual Studio 之类的工具，Visual Studio 带有适用于 [Docker](https://www.docker.com/) 的工具。 Visual Studio 自动为 ASP.NET 应用程序和 Windows 容器映像选择智能默认值。 这些工具提供快速内部循环，并提供将容器迁移到 Azure 的快速途径。 部署到多个环境中时，敏捷性得到提高。
然后，要迁移到生产环境，可以将 Windows 容器部署到[用于容器的 Azure Web 应用](https://azure.microsoft.com/services/app-service/containers/)、[Azure 容器实例 (ACI)](https://azure.microsoft.com/services/container-instances/)；如果更希望采用 IaaS 方法，还可部署到具有 Windows Server 2016 和容器的 Azure VM。 对于更复杂的多容器应用程序，请考虑使用 [Azure Kubernetes 服务 (AKS/ACS)](https://azure.microsoft.com/services/container-service/) 等业务流程协调程序。

初次更新期间，还可以从云中添加资产，如使用如下工具进行监视：[Azure Application Insights](/azure/application-insights/app-insights-overview)适用于应用生命周期且带 [Azure DevOps 服务](https://azure.microsoft.com/services/devops/)的 CI/CD 管道，以及 Azure 中提供的许多其他数据资源服务。 例如，可以修改单片 Web 应用，该应用最初使用传统 [ASP.NET Web 窗体](https://www.asp.net/web-forms)或 [ASP.NET MVC](https://www.asp.net/mvc) 开发，但现在使用 Windows 容器部署。 使用 Windows 容器时，还需要将数据迁移到 [Azure SQL 数据库托管实例](/azure/sql-database/)中的数据库，但不需要更改应用程序的核心体系结构。

- **云本机**：如上所述，在以大型复杂应用程序为目标，由多个独立开发团队独立开发和部署不同的微服务时，应考虑架构 [云本机](https://www.gartner.com/doc/3181919/architect-design-cloudnative-applications)应用程序。 此外，还有一个原因是每个微服务有精细化且独立的可扩展性。 这种体系结构方法面临着非常重要的挑战和复杂性，但使用云 PaaS、[Azure Kubernetes 服务 (AKS/ACS)](https://azure.microsoft.com/services/container-service/)（托管 Kubernetes）等业务流程协调程序和 [Azure Functions](https://azure.microsoft.com/services/functions/) 构建无服务器方法，可大大简化工作。 以上所有方法（如微服务和无服务器）通常需要架构云并编写新代码，新代码应适用于特定的 PaaS 平台或符合特定的体系结构（如微服务）。

图 1-3 显示了每个成熟度级别可使用的内部技术：

![每个更新成熟度级别可使用的内部技术](./media/image1-3.png)

**图 1-3**。 每个更新成熟度级别可使用的内部技术

## <a name="lift-and-shift-scenario"></a>直接迁移方案

对于直接迁移，请记住，在应用程序方案中可以使用很多不同的直接迁移变体。 如果仅重新托管应用程序，则可能拥有类似于图 1-4 中所示的方案，在此方案中，仅对应用程序和数据库服务器使用云中的 VM。

![云中纯 IaaS 方案的示例](./media/image1-4.png)

**图 1-4**。 云中纯 IaaS 方案的示例

## <a name="modernization-scenarios"></a>现代化方案

对于现代化方案，你可能拥有一个纯云优化的应用程序，该应用程序仅使用该成熟度级别的元素。 或者，你可能拥有中间状态的应用程序，该应用程序同时使用云基础结构就绪级别的元素以及云优化级别的其他元素（“挑选”或混合模型），如图 1-5。

![“挑选”方案示例，使用 IaaS 数据库、DevOps 和容器化资产](./media/image1-5.png)

**图 1-5**。 “挑选”方案示例，使用 IaaS 数据库、DevOps 和容器化资产

对于要迁移的许多现有 .NET Framework 应用程序而言，理想的方案是迁移到云优化应用程序，以实现事半功倍。 使用这种方法，将来还可进一步实现云本机。 图 1-6 显示了一个示例。

![云优化应用方案示例，使用 Windows 容器和托管服务](./media/image1-6.png)

**图 1-6**。 云优化应用方案示例，使用 Windows 容器和托管服务

你可以通过为特定方案添加若干微服务来扩展现有的云优化应用程序。 这会将你部分移动到云本机级别，当前指南不重点介绍此部分。

## <a name="what-this-guide-does-not-cover"></a>本指南未涵盖的内容

本指南涵盖了一小部分特定的示例方案，如图 1-7 所示。 本指南仅着重介绍直接迁移方案，最终着重介绍云优化模型。 在云优化模型中，.NET Framework 应用程序使用 Windows 容器以及额外的组件（如监视和 CI/CD 管道）进行更新。 每个组件都是确保将应用程序更快地、敏捷地部署到云的基础。

![本指南不涉及云本机](./media/image1-7.png)

**图 1-7** 本指南不涉及云本机

本指南的重点具有一定的针对性。 本指南介绍了直接迁移现有 .NET 应用程序而不重新构建和更改代码的途径。 最终，介绍了如何实现应用程序云优化。

本指南不介绍如何创建云本机应用程序，例如如何进步成为微服务体系结构。 若要重新构建应用程序或创建基于微服务的全新应用程序，请参阅电子书 [.NET 微服务：适用于容器化 .NET 应用程序的体系结构](https://aka.ms/microservicesebook)。

### <a name="additional-resources"></a>其他资源

- **使用 Microsoft 平台和工具的容器化 Docker 应用程序生命周期**（可下载的电子书）\
  <https://aka.ms/dockerlifecycleebook>

- **.NET 微服务：适用于容器化 .NET 应用程序的体系结构**（可下载的电子书）\
  <https://aka.ms/microservicesebook>

- **使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序**（可下载电子书）\
  <https://aka.ms/webappebook>

## <a name="who-should-use-this-guide"></a>本指南的目标读者

本指南适用于以下开发者和解决方案架构师：希望更新基于 .NET Framework 的现有 ASP.NET Web 应用程序或 WCF 服务，以提高交付和发布应用程序的敏捷性。

本指南可能也对技术决策者有帮助，例如希望大概了解使用 Windows 容器和使用 Microsoft Azure 部署到云可以获得哪些好处的企业架构师或开发负责人/主管。

## <a name="how-to-use-this-guide"></a>如何使用本指南

本指南解释了需要更新现有应用程序的原因，以及使用 Windows 容器将应用移动到云能获得的具体好处。 本指南前几章的内容适用于架构师和技术决策者，他们只需大概了解，不需要实现和技术分步细节。

本指南最后一章介绍重点针对特定部署方案的多个演练。 本指南提供了简要的演练版本，概况各种方案并突出其优点。 完整演练深入到了设置和实现细节，并作为一组 [Wiki 文章](https://github.com/dotnet-architecture/eShopModernizing/wiki)在相同的公共 [GitHub 存储库](https://github.com/dotnet-architecture/eShopModernizing)中发布，该存储库中还存放着相关示例应用（将在下一节讨论）。 关注实现细节的开发者和架构师将对最后一章以及 GitHub 上的分步 Wiki 演练更感兴趣。

## <a name="sample-apps-for-modernizing-legacy-apps-eshopmodernizing"></a>用于更新旧版应用的示例应用：eShopModernizing

GitHub 上的 [EShopModernizing](https://github.com/dotnet-architecture/eShopModernizing) 存储库提供了两个示例应用程序，用以模拟旧版单片 Web 应用程序。 第一个 Web 应用是用 ASP.NET MVC 开发的；第二个 Web 应用是用 ASP.NET Web Forms 开发的；三个应用是一个 N 层应用，其 WinForms 客户端桌面应用使用 WCF 服务后端。 这些应用都基于传统 .NET Framework。 这些示例应用不使用 .NET Core 或 ASP.NET Core，它们是要更新的现有/旧版 .NET Framework 应用程序。

这些示例应用都有第二个版本，它们使用现代化代码，十分直接。 两个应用版本之间最重要的差异是第二个版本使用 Windows 容器作为部署选项。 第二个版本中还新增了一些组件，如用于管理映像的 Azure 存储 Blobs、管理安全的 Azure Active Directory，以及用于监视和审核应用程序的 Azure Application Insights。

## <a name="send-your-feedback"></a>提供你的反馈

我们创作本指南，是为了帮助你了解改进和现代化现有 .NET Web 应用程序的选项。 本指南以及相关示例应用程序不断更新。 欢迎提供反馈意见！ 如有关于本指南的改进建议，请将其发送到 [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com?subject=Feedback%20for%20.NET%20Container%20&%20Microservices%20Architecture%20book)。

>[!div class="step-by-step"]
>[下一页](lift-and-shift-existing-apps-azure-iaas.md) <!-- Next Chapter -->
