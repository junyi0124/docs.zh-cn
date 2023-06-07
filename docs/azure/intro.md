---
title: Azure 和 .NET 入门
description: 学习了解 Azure 和 .NET 所需的基础知识。
ms.date: 06/20/2020
ms.openlocfilehash: e4a071e302247332cdc98c1aabf595cb4f8e2eff
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171567"
---
# <a name="introduction-to-azure-and-net"></a>Azure 和 .NET 简介

本文档概述的重要概念和服务是 .NET 开发人员使用 Azure 服务来完成应用开发入门所应该熟悉的。

## <a name="key-concepts"></a>主要概念

**Azure 帐户**：Azure 帐户是用于登录 Azure 服务（例如 [Azure 门户](https://portal.azure.com)或 [Cloud Shell](https://shell.azure.com)）的凭据。 如果没有 Azure 帐户，可以[免费创建一个](https://azure.microsoft.com/free/dotnet/)。

**Azure 订阅**：订阅是创建 Azure 资源时使用的计费计划。 订阅可以是个人订阅，也可以是由公司管理的企业订阅。 Azure 帐户可以与多个订阅相关联。 在这种情况下，请确保在创建资源时选择正确的订阅。 有关详细信息，请参阅[了解帐户、订阅和计费](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)。

> [!TIP]
> 如果有 Visual Studio 订阅，请[激活每月的 Azure 信用额度](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)。

资源组：可以通过资源组将 Azure 资源组织到组中进行管理。 在 Azure 中创建的资源存储在资源组中，这类似于将文件保存在计算机上的文件夹中。

**承载**：若要在 Azure 中运行代码，需将其承载在支持执行用户提供的代码的服务中。

**托管服务**：Azure 提供一些服务，你可以在其中向 Azure 提供数据或信息，然后 Azure 的实现会采取相应的措施。 Azure Blob 存储就是一个示例，你可以在其中提供文件，Azure 会处理其读取、写入和持久保存事项。

用于 .NET 的 Azure SDK 统称为你在项目中安装的 [NuGet 包](packages.md)，可提供 Azure 服务的各种交互和功能。 这些包还包括用于预配和管理资源的管理库。

## <a name="choosing-a-hosting-option"></a>选择承载选项

Azure 中的承载可以分为三个类别。

* **基础结构即服务 (IaaS)** ：可以使用 IaaS 预配所需的虚拟机以及关联的网络和存储组件。 然后将需要的任何软件和应用程序部署到这些 VM 上。 除了由 Microsoft 管理基础结构，该模型最接近传统的本地环境。 仍由你管理单独的 VM，包括操作系统、自定义软件和安全更新。

* **平台即服务 (PaaS)** ：PaaS 提供托管的承载环境，你可以在其中部署应用程序而无需管理 VM 或网络资源。 例如，指定实例计数而不是创建单独的 VM，服务将预配、配置并管理必需的资源。 Azure App Service 是 PaaS 服务的一个示例。
  
* **功能即服务 (FaaS)** ：通常称为无服务器计算。与 PaaS 相比，FaaS 更不需担心承载环境。 只需部署代码，服务便会自动运行它，无需创建计算实例并向其部署代码。 无需管理计算资源。 平台会根据处理流量的需要将代码无缝缩放到相应级别，你只需按代理运行情况付费。 Azure Functions 是 FaaS 服务。

通常情况下，应用程序越倾向于使用 FaaS 和 PaaS 模型，在云中运行的益处越大。 下面概述了 Azure 中的三个常用承载选项，以及何时选择它们。

* [Azure 应用服务](/azure/app-service/app-service-value-prop-what-is)：若要承载 Web 应用程序或服务，请先考虑应用服务。 有关应用服务和 ASP.NET、WCF 以及 ASP.NET Core 应用的入门，请参阅[在 Azure 中创建 ASP.NET Core Web 应用](/azure/app-service/app-service-web-get-started-dotnet)。

* [Azure Functions](/azure/azure-functions/functions-overview)：Azure Functions 适用于事件驱动型工作流。 示例包括：响应 Webhook、处理队列或 Blob 存储中的项，以及计时器。 若要开始使用 Azure Functions，请参阅[使用 Visual Studio 创建你的第一个函数](/azure/azure-functions/functions-create-your-first-function-visual-studio)。

* [Azure 虚拟机](/azure/virtual-machines/)：如果应用服务因为存在特定的依赖项而不符合你的承载现有应用程序的需求，则最好是从虚拟机着手。 有关虚拟机和 ASP.NET 或 WCF 的入门，请参阅 [Deploy an ASP.NET app to an Azure virtual machine](https://tutorials.visualstudio.com/aspnet-vm/intro)（将 ASP.NET 应用部署到 Azure 虚拟机）。

> [!TIP]
> 有关选择服务的详细信息，请参阅[为应用程序选择 Azure 计算服务](/azure/architecture/guide/technology-choices/compute-decision-tree)。

## <a name="choose-a-data-storage-service"></a>选择数据存储服务

Azure 提供多种服务，方便你根据自己的需求存储数据。 .NET 开发人员最常用的数据服务包括：

* [Azure SQL 数据库](/azure/sql-database/)：若要将已经使用 SQL Server 的应用程序迁移到云，可以很自然地从 Azure SQL 数据库着手。 若要入门，请参阅[教程：使用 SQL 数据库在 Azure 中生成 ASP.NET 应用](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase)。

* [Azure Cosmos DB](/azure/cosmos-db/)：Azure Cosmos DB 是专为云设计的现代数据库。 启动新的应用程序时，如果该应用程序还没有特定的数据库依赖项，则应考虑使用 Azure Cosmos DB。 对于侧重于以下要求的新 Web、移动、游戏和 IoT 应用程序而言，Cosmos DB 是一个不错的选择：自动缩放、可预测的性能、快速响应时间，以及查询无架构数据的能力。 若要开始使用，请参阅[快速入门：使用 SQL API 和 Azure 门户生成包含 Azure Cosmos DB 的 .NET Web 应用](/azure/cosmos-db/create-sql-api-dotnet)。

* [Azure Blob 存储](/azure/storage/)：Azure Blob 存储经过优化，适用于存储和检索大型二进制对象，例如图像、文件和流。 使用对象存储可以管理极大量的非结构化数据。 若要开始使用，请参阅[快速入门：使用 .NET 在对象存储中创建 blob](/azure/storage/blobs/storage-quickstart-blobs-dotnet)。

> [!TIP]
> 有关详细信息，请参阅[选择适当的数据存储](/azure/architecture/guide/technology-choices/data-store-overview)。

## <a name="connect-to-azure-services"></a>连接到 Azure 服务

如果使用 Visual Studio，可以将对某些 Azure 服务的支持添加到项目。 使用 Visual Studio 中的“连接的服务”对话框，可以方便地将所有所需的引用、连接代码和配置设置添加到项目。 默认情况下会支持一些常用的 Azure 服务，如[存储](/azure/vs-azure-tools-connected-services-storage)、[Azure Active Directory](/azure/active-directory/develop/vs-active-directory-add-connected-service) 身份验证、[Azure Key Vault](/azure/key-vault/vs-key-vault-add-connected-service) 和[认知服务](/azure/cognitive-services/)（如[计算机视觉](/azure/cognitive-services/computer-vision/vs-computer-vision-connected-service)）。 更多服务（包括第三方服务）可在 [Visual Studio Marketplace](https://marketplace.visualstudio.com/search?term=connected%20service&target=VS&category=Tools&vsVersion=&subCategory=All&sortBy=Relevance) 中作为扩展获得。

## <a name="using-the-azure-sdk-for-net"></a>使用用于 .NET 的 Azure SDK

如果使用用于 .NET 的 Azure SDK 来访问或管理 Azure 资源，请注意以下事项：

* **身份验证**：SDK 中的许多库使用常用的身份验证基础结构，而某些库则使用特定于其所使用的服务的身份验证机制。 有关详细信息，请参阅[使用用于 .NET 的 Azure SDK 进行身份验证](authentication.md)。
* **日志记录**：如果支持，客户端库则能够记录客户端库操作。 有关详细信息，请参阅[使用用于 .NET 的 Azure SDK 进行日志记录](logging.md)。
* **REST API**：用于 .NET 的 Azure SDK 是构建在 [Azure REST API](/rest/api/azure/) 基础上的一种抽象。 如果需要，可以使用 Azure REST API 来替代用于 .NET 的 Azure SDK，或者将两者结合使用。

## <a name="diagnosing-problems-in-the-cloud"></a>在云中诊断问题

将应用程序部署到 Azure 后，可能会遇到在开发环境中没有问题而在 Azure 中却有问题的情况。 诊断问题时，可以从以下两个方面着手：

* **从 Visual Studio 进行远程调试**：大多数 Azure 计算服务（包括本文档中讨论的服务）可以使用 Visual Studio 进行远程调试，并且可以获取日志。 若要通过应用程序探索 Visual Studio 的功能，请在 Visual Studio 的快速启动工具栏（位于右上角）中键入“Cloud Explorer”，以便打开 Cloud Explorer 工具窗口，然后在树目录中找到应用程序。 有关详细信息，请参阅[使用 Visual Studio 对 Azure 应用服务中的 Web 应用进行故障排除](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug)。

* **Application Insights**：[Application Insights](/azure/application-insights/) 是一种完备的应用程序性能监视 (APM) 解决方案，可以从应用程序中自动捕获诊断数据、遥测数据和性能数据。 若要开始收集应用的诊断数据，请参阅[开始监视 ASP.NET Web 应用程序](/azure/application-insights/quick-monitor-portal)。

## <a name="next-steps"></a>后续步骤

* [将第一个 ASP.NET Core Web 应用部署到 Azure](/azure/app-service/app-service-web-get-started-dotnet)
* [了解在用于 .NET 的 Azure SDK 中进行的身份验证](authentication.md)
* [Diagnose errors in your cloud apps](https://devblogs.microsoft.com/aspnet/diagnosing-errors-on-your-cloud-apps/)（诊断云应用中的错误）
* 下载免费电子书：[Azure Quick Start Guide for .NET Developers](https://www.microsoft.com/net/download/thank-you/azure-quick-start-ebook)（面向 .NET 开发人员的 Azure 快速入门指南）
