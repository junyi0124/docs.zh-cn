---
title: 承载数据服务（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, configuring
- WCF Data Services, Windows Communication Foundation
ms.assetid: b48f42ce-22ce-4f8d-8f0d-f7ddac9125ee
ms.openlocfilehash: 5dfa1d9f02f660b55ecf6598ef5012174a1ba853
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172588"
---
# <a name="hosting-the-data-service-wcf-data-services"></a>承载数据服务（WCF 数据服务）

通过使用 WCF 数据服务，你可以创建一个服务，该服务将数据作为 Open Data Protocol (OData) 源公开。 此数据服务定义为从 <xref:System.Data.Services.DataService%601> 继承的类。 此类提供处理请求消息、对数据源执行更新以及根据 OData 生成响应消息所需的功能。 但是，数据服务无法针对传入的 HTTP 请求绑定到网络套接字并进行侦听。 对于这一必需的功能，数据服务依赖于宿主计算机。

 数据服务主机可代表数据服务执行以下任务：

- 侦听请求并将这些请求路由到数据服务。

- 针对每个请求创建数据服务的一个实例。

- 请求数据服务处理传入的请求。

- 代表数据服务发送响应。

 为了简化数据服务的承载，WCF 数据服务旨在与 Windows Communication Foundation (WCF) 集成。 数据服务提供一个默认 WCF 实现，充当 ASP.NET 应用程序中的数据服务主机。 因此，您可以通过以下方式之一承载数据服务：

- 在 ASP.NET 应用程序中。

- 在支持自承载 WCF 服务的托管应用程序中。

- 在其他某些自定义数据服务宿主中。

## <a name="hosting-a-data-service-in-an-aspnet-application"></a>在 ASP.NET 应用程序中承载数据服务

使用 Visual Studio 2015 中的 " **添加新项** " 对话框在 ASP.NET 应用程序中定义数据服务时，该工具会在项目中生成两个新文件。 第一个文件的扩展名为 `.svc`，并指示 WCF 运行时如何实例化数据服务。 下面是在完成 [快速入门](quickstart-wcf-data-services.md)时创建的 Northwind 示例数据服务的此文件示例：

```aspx-csharp
<%@ ServiceHost Language="C#"
    Factory="System.Data.Services.DataServiceHostFactory,
            System.Data.Services, Version=4.0.0.0,
            Culture=neutral, PublicKeyToken=b77a5c561934e089"
    Service="NorthwindService.Northwind" %>
```

 此指令通知应用程序使用 <xref:System.Data.Services.DataServiceHostFactory> 类为命名数据服务类创建服务宿主。

 `.svc` 文件的代码隐藏页包含作为数据服务自身的实现的类，对于 Northwind 示例数据服务，该数据服务的定义如下：

 [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
 [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

 由于数据服务的行为与 WCF 服务类似，因此数据服务与 ASP.NET 集成并遵循 WCF Web 编程模型。 有关详细信息，请参阅 [Wcf 服务和 ASP.NET](../../wcf/feature-details/wcf-services-and-aspnet.md) 和 [Wcf Web HTTP 编程模型](../../wcf/feature-details/wcf-web-http-programming-model.md)。

## <a name="self-hosted-wcf-services"></a>自承载 WCF 服务

 由于它包含 WCF 实现，因此 WCF 数据服务支持作为 WCF 服务自承载数据服务。 服务可以在任何 .NET Framework 的应用程序中自承载，如控制台应用程序。 继承自 <xref:System.Data.Services.DataServiceHost> 的 <xref:System.ServiceModel.Web.WebServiceHost> 类用于实例化位于特定地址的数据服务。

 自承载功能可用于开发和测试目的，因为通过此功能更易于部署服务和解决服务问题。 但是，这种托管并不提供 ASP.NET 或 (IIS) Internet Information Services 提供的高级宿主和管理功能。 有关详细信息，请参阅 [托管应用程序中的托管](../../wcf/feature-details/hosting-in-a-managed-application.md)。

## <a name="defining-a-custom-data-service-host"></a>定义自定义数据服务主机

 对于 WCF 宿主实现过于受限的情况，您还可以为数据服务定义自定义宿主。 实现 <xref:System.Data.Services.IDataServiceHost> 接口的任何类都可用作数据服务的网络宿主。 自定义宿主必须实现 <xref:System.Data.Services.IDataServiceHost> 接口，并且能够承担数据服务主机的以下基本责任：

- 向数据服务提供服务根路径。

- 处理请求，并向相应的 <xref:System.Data.Services.IDataServiceHost> 成员实现响应标头信息。

- 处理由数据服务引发的异常。

- 验证查询字符串中的参数。

## <a name="see-also"></a>请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [将数据公开为服务](exposing-your-data-as-a-service-wcf-data-services.md)
- [配置数据服务](configuring-the-data-service-wcf-data-services.md)
