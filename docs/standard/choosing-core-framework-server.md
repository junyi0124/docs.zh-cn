---
title: 为服务器应用选择 .NET 5 或 .NET Framework
description: 可帮助你确定在生成服务器应用时要使用的 .NET 实现的指南。
author: cartermp
ms.date: 10/06/2020
ms.openlocfilehash: d9dce0343f9d37e976472b818e896a5b0a661e76
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160446"
---
# <a name="net-5-vs-net-framework-for-server-apps"></a>适用于服务器应用的 .NET 5 与 .NET Framework

有两种支持的 .NET 实现可用于生成服务器端应用：.NET Framework 和 .NET 5（包括 .NET Core）。 这两者共享许多相同的组件，可在它们之间共享代码。 但两者之间存在根本的差异，可根据需要实现的目标进行选择。 本文介绍了在何种情况下进行选择。

在以下情况，对服务器应用程序使用 .NET 5：

- 用户有跨平台需求。
- 你正在以微服务为目标。
- 你正在使用 Docker 容器。
- 需要高性能和可扩展的系统。
- 需按应用程序提供并行的 .NET 版本。

在以下情况，对服务器应用程序使用 .NET Framework ：

- 应用当前使用 .NET Framework（建议扩展而不是迁移）。
- 应用使用不可用于 .NET 5 的第三方 .NET 库或 NuGet 包。
- 应用使用不可用于 .NET 5 的 .NET 技术。
- 应用使用不支持 .NET 5 的平台。

## <a name="when-to-choose-net-5"></a>选择 .NET 5 的情形

以下各部分对前面提到的选择 .NET 5 的原因进行了进一步说明。

### <a name="cross-platform-needs"></a>跨平台需求

如果应用程序（Web/服务）需要在多个平台（Windows、Linux 和 macOS）上运行，请使用 .NET 5。

.NET 5 作为开发工作站支持前面提到的操作系统。 Visual Studio 提供了适用于 Windows 和 macOS 的集成开发环境 (IDE)。 还可使用运行于 macOS、Linux 和 Windows 上的 Visual Studio Code。 Visual Studio Code 支持 .NET 5，包括 IntelliSense 和调试。 大多数第三方编辑器（如 Sublime、Emacs 和 VI）都可搭配 .NET 5 使用。 这些第三方编辑器可使用 [Omnisharp](https://www.omnisharp.net/) 获取编辑器 IntelliSense。 也可不使用任何代码编辑器，直接使用适用于所有支持平台的 [.NET Core CLI](../core/tools/index.md)。

### <a name="microservices-architecture"></a>微服务体系结构

微服务体系结构允许跨服务边界组合使用技术。 通过这种技术组合，可逐步接受 .NET 5 作为能与其他微服务或服务搭配使用的新微服务。 例如，可组合使用微服务或使用 .NET Framework、Java、Ruby 或其他单片技术开发的服务。

可用的基础结构平台有很多。 [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/)，设计用于大型和复杂微服务系统 。 [Azure App Service](https://azure.microsoft.com/services/app-service/)，很适合用于无状态微服务。 基于 Docker 的微服务备选方案适合任何一种微服务方法，这部分内容将在[容器](#containers)部分进行说明。 所有这些平台都支持 .NET 5，是托管微服务的理想选择。

有关微服务体系结构的详细信息，请参阅 [.NET 微服务 - 适用于容器化 .NET 应用程序的体系结构](../architecture/microservices/index.md)。

### <a name="containers"></a>容器

容器通常与微服务体系结构结合使用。 还可使用容器将遵循任何体系结构模式的 Web 应用或服务容器化。 可在 Windows 容器上使用 .NET Framework，但 .NET 5 的模块化和轻型性质使之成为容器的更佳选择。 在创建和部署容器时，使用 .NET 5 时容器的映像大小要远小于使用 .NET Framework 时的大小。 例如，因为它是跨平台的，所以可将服务器应用部署到 Linux Docker 容器。

Docker 容器可托管在自己的 Linux 或 Windows 基础结构中，或托管在 [Azure Kubernetes 服务](https://azure.microsoft.com/services/kubernetes-service/)等云服务中。 Azure Kubernetes 服务可管理、协调和缩放云中基于容器的应用程序。

### <a name="high-performance-and-scalable-systems"></a>高性能和可扩展的系统

如果系统需要最佳的性能和可伸缩性，.NET 5 和 ASP.NET Core 是最佳的选择。 Windows Server 和 Linux 的高性能服务器运行时使 .NET 成为 [TechEmpower 基准](https://www.techempower.com/benchmarks/#hw=ph&test=plaintext)上执行最佳的 Web 框架。

性能和可伸缩性对微服务体系结构尤为重要，体系结构中可能正在运行数百个微服务。 借助 ASP.NET Core，系统运行的服务器/虚拟机 (VM) 数要低得多。 减少服务器/VM 后可节省基础结构和托管成本。

### <a name="side-by-side-net-versions-per-application-level"></a>按应用程序级别并行安装 .NET 版本

若要安装含不同 .NET 版本上的依赖项的应用程序，建议使用 NET 5。 .NET 5 支持在同一计算机上并行安装不同版本的 .NET 5 运行时。 并行安装允许在同一服务器上使用多个服务，每个服务位于其相应的 .NET 5（或者 .NET Core 2.1 或 3.1）版本上。 这还可在应用程序升级和 IT 运营时降低风险、节省成本。

.NET Framework 不支持并行安装。 它是一个 Windows 组件，一次只能有一个版本存在于计算机上。 .NET Framework 的每个版本均替换之前的版本。 如果安装面向 .NET Framework 更高版本的新应用，则可能会中断计算机上运行的现有应用，因为替换了之前的版本。

## <a name="when-to-choose-net-framework"></a>选择 .NET Framework 的情形

.NET 5 对新应用程序和应用程序模式特别有用。 但是在很多现有方案中依然会自然而然地选择 .NET Framework，并且对于所有服务器应用程序，.NET Framework 不会被 .NET 5 代替。

### <a name="current-net-framework-applications"></a>现有的 .NET Framework 应用程序

在大多数情况下，不需要将现有应用程序迁移到 .NET 5。 相反，若要扩展现有的应用程序（例如，在 ASP.NET Core 中写入新的 Web 服务），建议使用 .NET 5。

### <a name="third-party-libraries-or-nuget-packages-not-available-for-net-5"></a>不可用于 .NET 5 的第三方库或 NuGet 包

通过 .NET Standard 可跨各种 .NET 实现（包括 .NET 5）共享代码。 使用 .NET Standard 2.0，兼容性模式允许 .NET Standard 和 .NET 5 项目引用 .NET Framework 库。 有关详细信息，请参阅[对 .NET Framework 库的支持](whats-new/whats-new-in-dotnet-standard.md#support-for-net-framework-libraries)。

仅在以下情况下需要使用 .NET Framework：库或 NuGet 包使用 .NET Standard 或 .NET 5 中不提供的技术。

### <a name="net-technologies-not-available-for-net-5"></a>不可用于 .NET 5 的 .NET 技术

某些 .NET Framework 技术在 .NET 5 中不可用。 以下列表显示无法在 .NET 5 中找到的最常见技术：

- ASP.NET Web 窗体应用程序：ASP.NET Web 窗体仅在.NET Framework 中可用。 ASP.NET Core 不能用于 ASP.NET Web 窗体。

- ASP.NET 网页应用程序：ASP.NET 网页未包含在 ASP.NET Core 中。

- WCF 服务的实现。 虽然 [WCF 客户端库](https://github.com/dotnet/wcf)可从 .NET 5 使用 WCF 服务，WCF 服务器实现目前只在 .NET Framework 上可用。

- 工作流相关的服务：Windows Workflow Foundation (WF)、工作流服务（WCF + 单个服务中的 WF）和 WCF Data Services（以前称为“ADO.NET Data Services”）仅在 .NET Framework 上可用。

- 语言支持：.NET 5 目前支持 Visual Basic 和 F#，但不是所有项目类型都支持。 有关支持的项目模板列表，请参阅 [dotnet new 的模板选项](../core/tools/dotnet-new.md#arguments)。

有关详细信息，请参阅[在 .NET 5 中不可用的 .NET Framework 技术](../core/porting/net-framework-tech-unavailable.md)。

### <a name="platform-doesnt-support-net-5"></a>平台不支持 .NET 5

某些 Microsoft 或第三方平台不支持 .NET 5。 某些 Azure 服务提供尚不可用于 .NET 5 的 SDK。 在这种情况下，可使用等效的 REST API（而不是客户端 SDK）。

## <a name="see-also"></a>请参阅

- [在 ASP.NET 和 ASP.NET Core 之间进行选择](/aspnet/core/choose-aspnet-framework)
- [面向 .NET Framework 的 ASP.NET Core](/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-2.2&preserve-view=true#aspnet-core-targeting-net-framework)
- [目标框架](frameworks.md)
- [.NET 简介](../core/introduction.md)
- [从 .NET Framework 移植到 .NET 5](../core/porting/index.md)
- [.NET 和 Docker 简介](../core/docker/introduction.md)
- [.NET 组件概述](components.md)
- [.NET 微服务 - 适用于容器化 .NET 应用程序的体系结构](../architecture/microservices/index.md)
