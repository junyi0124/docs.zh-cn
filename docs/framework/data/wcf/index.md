---
title: WCF 数据服务 4.5
description: 了解 WCF 数据服务，它是一个 .NET Framework 组件，它支持使用 REST 语义公开和使用数据的服务。
ms.date: 03/30/2017
helpviewer_keywords:
- Astoria
- WCF Data Services, getting started
ms.assetid: 73d2bec3-7c92-4110-b905-11bb0462357a
ms.openlocfilehash: c36967236c40efbf432d554c3f551aea22cfb148
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90549674"
---
# <a name="wcf-data-services-45"></a>WCF 数据服务 4.5

WCF 数据服务 (以前称为 "ADO.NET Data Services" ) 是 .NET Framework 的一个组件，它使你能够通过使用 [具象状态传输 OPEN DATA PROTOCOL REST (](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)的语义，来创建使用)  (OData) 在 Web 或 intranet 上公开和使用数据的服务。 OData 将数据公开为可通过 URI 进行寻址的资源。 通过使用标准 HTTP 谓词 GET、PUT、POST 和 DELETE 访问和更改数据。 OData 使用 [实体数据模型](../adonet/entity-data-model.md) 的实体关系约定将资源公开为通过关联相关的实体集。

WCF 数据服务使用 OData 协议对资源进行寻址和更新。 通过这种方式，你可以从支持 OData 的任何客户端访问这些服务。 OData 使你可以通过使用众所周知的传输格式请求数据并将数据写入资源： Atom，一组用于以 XML 格式交换和更新数据的标准，并 JavaScript 对象表示法 (JSON) ，这是在 AJAX 应用程序中广泛使用的基于文本的数据交换格式。

WCF 数据服务可以将源自各种源的数据作为 OData 源公开。 Visual Studio 工具通过使用 ADO.NET 实体框架数据模型，使你可以更轻松地创建基于 OData 的服务。 还可以基于公共语言运行时 (CLR) 类，甚至是后期绑定或未类型化的数据来创建 OData 源。

WCF 数据服务还包括一组客户端库，一个用于一般 .NET Framework 客户端应用程序，另一个专用于基于 Silverlight 的应用程序。 在从 .NET Framework 和 Silverlight 之类的环境访问 OData 源时，这些客户端库提供了基于对象的编程模型。

## <a name="where-should-i-start"></a>从何处开始？

根据你的兴趣，请考虑以下主题之一中的 WCF 数据服务入门。

我希望直接开始使用...

- [快速入门](quickstart-wcf-data-services.md)

- [入门](getting-started-with-wcf-data-services.md)

只显示一些代码 .。。

- [快速入门](quickstart-wcf-data-services.md)

- [如何：执行数据服务查询](how-to-execute-data-service-queries-wcf-data-services.md)

- [如何：将数据绑定到 Windows Presentation Foundation 元素](bind-data-to-wpf-elements-wcf-data-services.md)

我想了解有关 OData 的详细信息 .。。

- [白皮书： OData 简介](https://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf)
- [Open Data Protocol 网站](https://www.odata.org/)
- [OData：SDK](https://www.odata.org/ecosystem/)

我想要查看端到端示例 .。。

- [WCF 数据服务快速入门](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/WCF%20Data%20Services%20Quickstart%20(OData%20Service%20and%20WPF%20Client))
- [OData SDK-示例代码](https://www.odata.org/ecosystem/#sdk)

它如何与 Visual Studio 集成？

- [生成数据服务客户端库](generating-the-data-service-client-library-wcf-data-services.md)

- [创建数据服务](creating-the-data-service.md)

- [实体框架提供程序](entity-framework-provider-wcf-data-services.md)

它的作用是什么？

- [概述](wcf-data-services-overview.md)

- [应用程序方案](application-scenarios-wcf-data-services.md)

我想要使用 LINQ .。。

- [查询数据服务](querying-the-data-service-wcf-data-services.md)

- [LINQ 注意事项](linq-considerations-wcf-data-services.md)

- [如何：执行数据服务查询](how-to-execute-data-service-queries-wcf-data-services.md)

我仍然需要了解更多信息 .。。

- [WCF Data Services Team Blog](/archive/blogs/astoriateam/)（WCF Data Services 团队博客）

- [资源](wcf-data-services-resources.md)

## <a name="in-this-section"></a>本节内容

[概述](wcf-data-services-overview.md)

概述 WCF 数据服务中可用的特性和功能。

[WCF 数据服务5.0 的新增功能](/previous-versions/dotnet/wcf-data-services/ee373845(v=vs.103))

介绍 WCF 数据服务的新功能，并支持新的 OData 功能。

[入门](getting-started-with-wcf-data-services.md)

介绍如何使用 WCF 数据服务公开和使用 OData 源。

[定义 WCF 数据服务](defining-wcf-data-services.md)

介绍如何创建和配置公开 OData 源的数据服务。

[WCF 数据服务客户端库](wcf-data-services-client-library.md)

介绍如何使用客户端库从 .NET Framework 客户端应用程序中使用 OData 源。

## <a name="see-also"></a>请参阅

- [Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)（表述性状态转移 (REST)）
