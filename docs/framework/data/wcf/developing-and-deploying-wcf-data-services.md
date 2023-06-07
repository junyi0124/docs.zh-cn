---
title: 开发和部署 WCF 数据服务
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, developing
- WCF Data Services, deploying
- deploying [WCF Data Services
- developing applications [WCF Data Services]
ms.assetid: 6557c0e3-5aea-4f6e-bc14-77ad317a168b
ms.openlocfilehash: 484505406701b52a2b80b95b718a23a2156aa22c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2020
ms.locfileid: "90556084"
---
# <a name="develop-and-deploy-wcf-data-services"></a>开发和部署 WCF 数据服务

本文提供有关开发和部署 WCF 数据服务的信息。 有关 WCF 数据服务的更多基本信息，请参阅 [入门](getting-started-with-wcf-data-services.md) 和 [概述](wcf-data-services-overview.md)。

## <a name="develop-wcf-data-services"></a>开发 WCF 数据服务

使用 WCF 数据服务创建支持 Open Data Protocol (OData) 的数据服务时，必须在开发过程中执行以下基本任务：

1. **定义数据模型**

     WCF 数据服务支持各种数据服务提供程序，这些提供程序使你能够基于来自各种数据源（从关系数据库到后期绑定数据类型）的数据定义数据模型。 有关详细信息，请参阅 [数据服务提供程序](data-services-providers-wcf-data-services.md)。

2. **创建数据服务**

     大多数基本数据服务公开一个从 <xref:System.Data.Services.DataService%601> 类继承的类和一个作为实体容器的命名空间限定名称的 `T` 类型。 有关详细信息，请参阅 [Defining WCF Data Services](defining-wcf-data-services.md)的信息。

3. **配置数据服务**

     默认情况下，WCF 数据服务禁用对由实体容器公开的资源的访问。 使用 <xref:System.Data.Services.DataServiceConfiguration> 接口可以配置对资源和服务操作的访问，指定支持的 OData 版本，还可以定义其他服务范围的行为，例如批处理行为或可在单个响应源中返回的最大实体数。 有关详细信息，请参阅 [配置数据服务](configuring-the-data-service-wcf-data-services.md)。

本文主要介绍如何使用 Visual Studio 开发和部署数据服务。 有关将数据公开为 OData 源的 WCF 数据服务提供的灵活性的信息，请参阅 [定义 WCF 数据服务](defining-wcf-data-services.md)。

### <a name="choose-a-development-web-server"></a>选择开发 Web 服务器

使用 Visual Studio 2015 将 WCF 数据服务开发为 ASP.NET 应用程序或 ASP.NET 网站时，可以选择在开发期间运行该数据服务的 Web 服务器。 以下 Web 服务器与 Visual Studio 集成，使您可以更轻松地在本地计算机上测试和调试数据服务。

1. **本地 IIS 服务器**

     如果创建的数据服务是在 Internet Information Services (IIS) 上运行的 ASP.NET 应用程序或 ASP.NET 网站，则建议使用本地计算机上的 IIS 开发和测试数据服务。 在 IIS 上运行数据服务更便于在调试过程中跟踪 HTTP 请求。 这还允许预先确定 IIS 访问数据服务所需的文件、数据库和其他资源所需的必要权限。 若要在 IIS 上运行数据服务，请确保已正确安装和配置了 IIS 和 Windows Communication Foundation (WCF) ，并在文件系统和数据库中授予对 IIS 帐户的访问权限。 有关详细信息，请参阅 [How to: Develop a WCF Data Service Running on IIS](how-to-develop-a-wcf-data-service-running-on-iis.md)。

    > [!NOTE]
    > 必须使用管理员权限运行 Visual Studio，才能启用开发环境以配置本地 IIS 服务器。

2. **Visual Studio 开发服务器**

     Visual Studio 包含一个内置 Web 服务器，即 Visual Studio 开发服务器，这是 ASP.NET 项目的默认 Web 服务器。 此 Web 服务器设计为在开发过程中在本地计算机上运行 ASP.NET 项目。 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)演示如何创建在 Visual Studio 开发服务器中运行的数据服务。

     使用 Visual Studio 开发服务器开发数据服务时，请注意以下限制：

    - 该服务器只能在本地计算机上访问。

    - 该服务器侦听 `localhost` 和特定端口，而非端口 80，后者是 HTTP 消息的默认端口。 有关详细信息，请参阅 [Visual Studio 中用于 ASP.NET Web 项目的 Web 服务器](/previous-versions/aspnet/58wxa9w5(v=vs.120))。

    - 该服务器在当前用户帐户的上下文中运行数据服务。 例如，如果作为管理员级别的用户运行，则在 Visual Studio 开发服务器中运行的数据服务将具有管理员级别的权限。 这会导致数据服务能够访问在部署到 IIS 服务器时无权访问的资源。

    - 该服务器不包含 IIS 的额外功能，如身份验证。

    - 此服务器无法处理分块 HTTP 流，在从数据服务访问大型二进制数据时，WCF 数据服务客户端默认发送此流。 有关详细信息，请参阅 [流式处理提供程序](streaming-provider-wcf-data-services.md)。

    - 此服务器在处理 URL 中的句点 (`.`) 字符时遇到问题，即使密钥值 WCF 数据服务支持此字符。

    > [!TIP]
    > 即使您可以使用 Visual Studio 开发服务器在开发过程中测试数据服务，您应该在部署到运行 IIS 的 Web 服务器之后再次对其进行测试。

3. **Azure 开发环境**

     Azure Tools for Visual Studio 包含一组集成的工具，用于在 Visual Studio 中开发 Azure 服务。 使用这些工具，可以开发可部署到 Azure 的数据服务，并且可以在部署之前在本地计算机上测试数据服务。 使用 Visual Studio 开发在 Azure 平台上运行的数据服务时，请使用这些工具。 有关安装这些工具的信息，请参阅 [Azure tools For Visual Studio 2015](../../../azure/vs2015-install.md)。 有关开发在 Azure 上运行的数据服务的详细信息，请参阅文章在 [azure 中部署 OData 服务](/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure)。

### <a name="development-tips"></a>开发提示

开发数据服务时，请注意以下事项：

- 如果计划对用户进行身份验证或限制特定用户的访问，请确定数据服务的安全要求。 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。

- HTTP 检查程序在调试数据服务时可能很有用，因为这样可以检查请求和响应消息的内容。 可以使用任何可显示原始数据包的网络数据包分析器检查发向数据服务的 HTTP 请求和来自数据服务的响应。

- 调试数据服务时，您可能需要从数据服务获取与常规操作过程中的错误有关的详细信息。 通过将 <xref:System.Data.Services.DataServiceConfiguration.UseVerboseErrors%2A> 中的 <xref:System.Data.Services.DataServiceConfiguration> 属性设置为 `true` 并将数据服务类的 <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A> 特性的 <xref:System.ServiceModel.Description.ServiceDebugBehavior> 属性设置为 `true`，可以从数据服务获取其他错误信息。 有关详细信息，请参阅 [调试后 WCF 数据服务](/archive/blogs/phaniraj/debugging-wcf-data-services)。 你还可以在 WCF 中启用跟踪，以查看 HTTP 消息层中出现的异常。 有关更多信息，请参见 [Configuring Tracing](../../wcf/diagnostics/tracing/configuring-tracing.md)。

- 数据服务通常开发为 ASP.NET 应用程序项目，但也可以在 Visual Studio 中将数据服务创建为 ASP.NET 网站项目。 有关这两种类型的项目之间的差异的信息，请参阅 [Web 应用程序项目与 Visual Studio 中](/previous-versions/aspnet/dd547590(v=vs.110))的网站项目。

- 使用 Visual Studio 中的 " **添加新项** " 对话框创建数据服务时，数据服务由 IIS 中的 ASP.NET 托管。 尽管 ASP.NET 和 IIS 是数据服务的默认宿主，但也支持其他宿主选项。 有关详细信息，请参阅 [承载数据服务](hosting-the-data-service-wcf-data-services.md)。

## <a name="deploy-wcf-data-services"></a>部署 WCF 数据服务

WCF 数据服务在选择承载数据服务的过程方面很灵活。 可以使用 Visual Studio 将数据服务部署到以下平台：

- **承载有 IIS 的 Web 服务器**

    将数据服务作为 ASP.NET 项目开发时，可以使用标准 ASP.NET 部署过程将其部署到 IIS Web 服务器。 Visual Studio 提供了以下 ASP.NET 部署技术，具体取决于承载要部署的数据服务的 ASP.NET 项目类型。

  - **ASP.NET Web 应用程序的部署技术**

    - [如何：在 Visual Studio 中创建 Web 部署包](/previous-versions/aspnet/dd465323(v=vs.110))

    - [如何：使用在 Visual Studio 中发布 One-Click 部署 Web 项目](/previous-versions/aspnet/dd465337(v=vs.110))

  - **ASP.NET 网站的部署技术**

    - [如何：通过 "复制网站" 工具复制网站文件](/previous-versions/aspnet/c95809c0(v=vs.100))

    - [如何：发布网站](/previous-versions/aspnet/20yh9f1b(v=vs.100))

    - [演练：使用 XCOPY 部署 ASP.NET Web 应用程序](/previous-versions/aspnet/f735abw9(v=vs.100))

     有关 ASP.NET 应用程序的部署选项的详细信息，请参阅 [Visual Studio 和 ASP.NET 的 Web 部署概述](/previous-versions/aspnet/dd394698(v=vs.110))。

    > [!TIP]
    > 在尝试将数据服务部署到 IIS 之前，请确保已测试了向运行 IIS 的 Web 服务器的部署。 有关详细信息，请参阅 [How to: Develop a WCF Data Service Running on IIS](how-to-develop-a-wcf-data-service-running-on-iis.md)。

- **Azure**

     可以使用 [Azure Tools For Visual Studio](../../../azure/vs2015-install.md)将数据服务部署到 azure。 有关将数据服务部署到 Azure 的详细信息，请参阅 [在 azure 中部署 OData 服务](/archive/blogs/astoriateam/deploying-an-odata-service-in-windows-azure)。

### <a name="deployment-considerations"></a>部署注意事项

部署数据服务时，请注意以下事项：

- 当你部署使用实体框架提供程序访问 SQL Server 数据库的数据服务时，你可能还需要传播数据结构和/或数据。 Visual Studio 可以自动创建脚本 ( .sql 文件) 在目标数据库中执行此操作，并且这些脚本可以包含在 ASP.NET 应用程序的 Web 部署包中。 有关详细信息，请参阅 [如何：使用 Web 应用程序项目部署数据库](/previous-versions/dd465343(v=vs.100))。 对于 ASP.NET 网站，可以使用 Visual Studio 中的 " **数据库发布向导** " 来执行此操作。 有关详细信息，请参阅 [发布 SQL 数据库](/previous-versions/aspnet/bb907585(v=vs.100))。

- 由于 WCF 数据服务包含基本 WCF 实现，因此你可以使用 Windows Server AppFabric 来监视部署到在 Windows Server 上运行的 IIS 的数据服务。 有关使用 Windows Server AppFabric 监视数据服务的详细信息，请参阅 [使用 Windows Server appfabric 进行跟踪后 WCF 数据服务](/archive/blogs/rjacobs/tracking-wcf-data-services-with-windows-server-appfabric)。

## <a name="see-also"></a>另请参阅

- [承载数据服务](hosting-the-data-service-wcf-data-services.md)
- [WCF 数据服务的安全](securing-wcf-data-services.md)
- [定义 WCF 数据服务](defining-wcf-data-services.md)
