---
title: Azure 的开发过程
description: 使用 ASP.NET Core 和 Azure 构建新式 Web 应用程序 | Azure 的开发过程
author: ardalis
ms.author: wiwagn
ms.date: 12/01/2020
ms.openlocfilehash: 2706a4091565e6f3cb795acf031a238ae55a1068
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851622"
---
# <a name="development-process-for-azure"></a>Azure 的开发过程

> “凭借云，个体和小型企业可轻松无忧地立即建立企业级服务。”   
> \- Roy Stephan 

## <a name="vision"></a>愿景

> 使用 Visual Studio、dotnet CLI、Visual Studio Code 或所选用的编辑器，按照自己喜欢的方式开发出设计优良的 ASP .NET Core 应用程序。 

## <a name="development-environment-for-aspnet-core-apps"></a>ASP.NET Core 应用的开发环境

### <a name="development-tools-choices-ide-or-editor"></a>开发工具选择：IDE 或编辑器

无论你更青睐内容丰富、功能强大的 IDE 还是灵活轻量的编辑器，Microsoft 都可为你提供用于开发 ASP.NET Core 应用程序的工具。

**Visual Studio 2019。** Visual Studio 2019 是用于开发适用于 ASP.NET Core 的应用程序的同类最佳 IDE。 它提供了多种可提高开发人员生产率的功能。 可以使用它来开发应用程序，然后分析其性能和其他特征。 借助集成的调试程序，你可以暂停代码执行，并在运行时在代码中来回切换。 借助内置的测试运行程序，你可以组织测试及其结果，甚至可以在编写代码时执行实时单元测试。 使用 Live Share，可以与其他开发人员进行实时协作，并通过网络无缝共享代码会话。 准备就绪后，Visual Studio 包括了将应用程序发布到 Azure 所需的所有内容或任何可托管它的位置。

[下载 Visual Studio 2019](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)

Visual Studio Code 和 dotnet CLI（适用于 Mac、Linux 和 Windows 的跨平台工具）。 如果更青睐支持任何开发语言的轻量级跨平台编辑器，可以使用 Microsoft Visual Studio Code 和 the dotnet CLI。 这些产品提供简单但可靠的体验，可以简化开发人员工作流。 此外，Visual Studio Code 支持适用于 C\# 和 Web 开发的扩展，在该编辑器内提供智能感知和快捷任务。

[下载 .NET SDK](https://dotnet.microsoft.com/download)

[下载 Visual Studio Code](https://code.visualstudio.com/download)

## <a name="development-workflow-for-azure-hosted-aspnet-core-apps"></a>Azure 托管型 ASP.NET Core 应用的开发工作流

应用程序开发生命周期始于每位开发人员的计算机，开发人员使用其首选语言对应用进行编码和本地测试。 开发人员可选择其青睐的源代码管理系统，并可使用生成服务器或基于内置 Azure 功能配置持续集成 (CI) 和/或持续交付/部署 (CD)。

若要开始使用 CI/CD 开发 ASP.NET Core 应用程序，可使用 Azure DevOps Services 或组织自身的 Team Foundation Server (TFS)。

### <a name="initial-setup"></a>初始设置

若要为应用创建发布管道，需要具备源代码管理中的应用程序代码。 设置一个本地存储库，并将其连接到团队项目中的远程存储库。 请按照以下说明执行操作：

- [使用 Git 和 Visual Studio 分享代码](/azure/devops/git/share-your-code-in-git-vs)或

- [使用 TFVC 和 Visual Studio 分享代码](/azure/devops/tfvc/share-your-code-in-tfvc-vs)

创建要在其中部署应用程序的 Azure 应用服务。 转至 Azure 门户上的“应用服务”选项卡，创建一个 Web 应用。 单击“+添加”，选择 Web 应用模板，单击“创建”并提供名称或其他详细信息。 该 Web 应用可通过 {name}.azurewebsites.net 进行访问。

![AzureWebApp](./media/image10-2.png)

**图 10-1.** 在 Azure 门户中创建新的 Azure 应用服务 Web 应用。

无论新代码何时提交至项目的源代码管理存储库，CI 生成过程均会执行自动生成。 由此过程你可获得即时反馈，知悉代码已生成（且理想情况下可通过自动测试），并且或许可进行部署。 此 CI 生成将生成一个 Web 部署包项目，并将其发布，以供 CD 进程使用。

[定义 CI 生成过程](/azure/devops/pipelines/ecosystems/dotnet-core)

请务必启用持续集成，从而使得无论何时团队成员提交新代码，系统均可将生成排队。 测试该生成，并验证其是否生成 Web 部署包作为其中一个项目。

生成成功后，CD 过程会将 CI 生成结果部署到 Azure Web 应用。 若要配置此步骤，请创建并配置一个版本，该版本将部署到 Azure 应用服务。

[部署 Azure web 应用](/azure/devops/pipelines/targets/webapp)

配置 CI/CD 管道后，即可更新 Web 应用，并将更新提交至源代码管理以进行部署。

### <a name="workflow-for-developing-azure-hosted-aspnet-core-applications"></a>开发 Azure 托管型 ASP.NET Core 应用程序的工作流

配置 Azure 帐户和 CI/CD 过程后，即可轻松开发 Azure 托管的 ASP.NET Core 应用程序。 生成 ASP.NET Core 应用并将其托管在 Azure 应用服务作为 Web 应用时，通常会采用以下基本步骤，如图 10-2 所示。

![EndToEndDevDeployWorkflow](./media/image10-3.png)

**图 10-2.** 构建 ASP.NET Core 应用以及将应用于托管于 Azure 的分步工作流

#### <a name="step-1-local-dev-environment-inner-loop"></a>步骤 1。 本地开发环境内循环

开发要部署到 Azure 的 ASP.NET Core 应用程序与开发其他程序并无不同。 使用惯用的本地开发环境，无论是 Visual Studio 2019、dotnet CLI、Visual Studio Code 还是个人首选的编辑器。 在准备将更改推送到共享源代码存储库前，可以编写代码、运行并调试更改、运行自动测试以及本地提交到源代码管理。

#### <a name="step-2-application-code-repository"></a>步骤 2。 应用程序代码存储库

无论何时准备与团队共享代码，均应将更改从本地源存储库中推送到团队共享源存储库。 如果一直在自定义分支中工作，此步骤通常涉及将代码合并到共享分支中（或许通过[拉取请求](/azure/devops/git/pull-requests)方式）。

#### <a name="step-3-build-server-continuous-integration-build-test-package"></a>步骤 3。 生成服务器：持续集成。 生成、测试、打包

向共享应用程序代码存储库进行新的提交时，生成服务器上会触发新的生成。 作为 CI 过程的一部分，该生成应充分编译应用程序，并运行自动测试以确定一切正常。 CI 过程的最终结果应是已可部署的打包版 Web 应用。

#### <a name="step-4-build-server-continuous-delivery"></a>步骤 4。 生成服务器：持续交付

生成成功后，CD 过程将选取产生的生成项目。 此过程将包括一个 Web 部署包。 生成服务器将此包部署到 Azure 应用服务，使用新创建的服务替换任何现有服务。 通常该步骤面向过渡环境，但是部分应用程序通过 CD 过程直接部署到生产。

#### <a name="step-5-azure-app-service-web-app"></a>步骤 5。 Azure 应用服务 Web 应用

部署后，ASP.NET Core 应用程序在 Azure 应用服务 Web 应用的上下文运行。 可使用 Azure 门户监视以及进一步配置该 Web 应用。

#### <a name="step-6-production-monitoring-and-diagnostics"></a>步骤 6。 生产监视和诊断

该 Web 应用运行时，可监视该应用程序的运行状况，并收集诊断和用户行为数据。 Application Insights 包含于 Visual Studio 中，为 ASP.NET 应用提供自动检测。 它可提供有关使用情况、异常、请求、性能和日志的信息。

## <a name="references"></a>参考资料

**构建 ASP.NET Core 应用并将其部署到 Azure**  
<https://docs.microsoft.com/azure/devops/build-release/apps/aspnet/build-aspnet-core>

>[!div class="step-by-step"]
>[上一页](test-asp-net-core-mvc-apps.md)
>[下一页](azure-hosting-recommendations-for-asp-net-web-apps.md)
