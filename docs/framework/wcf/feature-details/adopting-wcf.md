---
title: 采用 Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 49ba71e2-9468-4082-84c5-cf8daf95e34a
ms.openlocfilehash: 40a2eac1e282640f0df70a7eca16e3b2401c0764
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90546003"
---
# <a name="adopt-windows-communication-foundation"></a>采用 Windows Communication Foundation

你可以选择使用 Windows Communication Foundation (WCF) 进行新开发，同时继续维护使用 ASP.NET 开发的现有应用程序。 因为 WCF 旨在作为在任何方案中与使用 .NET Framework 构建的应用程序进行通信的最合适选择，因此它可以充当一种标准工具，用于以 ASP.NET 不能解决的各种软件通信问题。

新的 WCF 应用程序可以与现有的 ASP.NET Web 服务部署在同一台计算机上。 如果现有的 ASP.NET Web 服务使用版本2.0 之前的 .NET Framework 版本，则可以使用 ASP.NET IIS 注册工具将 .NET Framework 2.0 选择性地部署到要在其中托管新 WCF 应用程序的 IIS 应用程序。 该工具记录在 [ASP.NET Iis 注册工具 ( # A0) ](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90))上，并在 iis 6.0 管理控制台中内置了一个用户界面。

WCF 可用于将新功能添加到现有的 ASP.NET Web 服务，方法是将配置为在 ASP.NET 兼容模式下运行的 WCF 服务添加到 IIS 中的现有 ASP.NET Web 服务应用程序。 由于 ASP.NET 兼容模式，新的 WCF 服务的代码可使用类访问和更新与预先存在的 ASP.NET 代码相同的应用程序状态信息 <xref:System.Web.HttpContext> 。 这些应用程序还可以共享相同的类库。

WCF 客户端可以使用 ASP.NET Web 服务。 使用配置的 WCF 服务 <xref:System.ServiceModel.BasicHttpBinding> 可以由 ASP.NET Web 服务客户端使用。 ASP.NET Web 服务可与 WCF 应用程序共存，甚至可以使用 WCF 将功能添加到现有的 ASP.NET Web 服务。 假设 WCF 和 ASP.NET Web 服务可一起使用的所有方法都可以一起使用，只需在需要 WCF 提供的功能而不是 ASP.NET Web 服务时，才将 ASP.NET Web 服务迁移到 WCF。

即使在少数情况下，将代码从一种技术迁移到另一种技术也不是正确的方法。 采用新技术的原因是为了满足早期技术无法满足的新要求，在这种情况下，正确的做法是设计一个新的解决方案来满足新扩展的需求集。 新的设计将受益于您对现有系统所拥有的经验以及您自该系统被设计以来获得的智慧。 此外，新设计还可以利用新技术的所有功能，而不是在新平台上再现旧设计。 在对新设计的关键元素进行原型设计后，在新的系统内重复使用代码会变得更加容易。

## <a name="see-also"></a>请参阅

- [如何：检索元数据并实现兼容服务](how-to-retrieve-metadata-and-implement-a-compliant-service.md)
