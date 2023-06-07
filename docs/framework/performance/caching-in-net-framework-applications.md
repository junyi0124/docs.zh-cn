---
title: .NET Framework 应用程序中的缓存
description: 在 .NET 应用程序中使用缓存。 了解如何缓存数据、在 ASP.NET 应用程序中缓存或 WCF REST 服务，以及如何在 .NET 中扩展缓存。
ms.date: 03/30/2017
helpviewer_keywords:
- ASP.NET caching
- caching [.NET Framework]
- caching [ASP.NET]
ms.assetid: c4b47ee0-4b82-4124-9bce-818088385e34
ms.openlocfilehash: 5518151c26c528095ec91116b53e82e22e23d25f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235203"
---
# <a name="caching-in-net-framework-applications"></a>.NET Framework 应用程序中的缓存

缓存可以将数据存储在内存中以便快速访问。 再次访问数据时，应用程序可以从缓存获取数据，而不是从原始源检索数据。 这可改善性能和可伸缩性。 此外，数据源暂时不可用时，缓存可提供数据。

 .NET Framework 提供了缓存功能，可提高 Windows 客户端和服务器应用程序（包括 ASP.NET）的性能和可伸缩性。

> [!NOTE]
> 在 .NET Framework 3.5 及更早版本中，ASP.NET 提供了命名空间中的内存中缓存实现 <xref:System.Web.Caching> 。 在 .NET Framework 的以前版本中，仅在命名空间中提供缓存， <xref:System.Web> 因此需要依赖 ASP.NET 类。 在 .NET Framework 4 中，<xref:System.Runtime.Caching> 命名空间包含为 Web 和非 Web 应用程序设计的 API。

## <a name="caching-data"></a>缓存数据

 可使用 <xref:System.Runtime.Caching> 命名空间中的类来缓存信息。 此命名空间中的缓存类提供下列功能：

- 为创建自定义缓存实现提供基础的抽象类型。

- 具体的内存中对象缓存实现。

 抽象基缓存类 (<xref:System.Runtime.Caching.ObjectCache>) 定义以下缓存任务：

- 创建和管理缓存项。

- 指定过期和逐出信息。

- 触发响应缓存项更改而引发的事件。

 <xref:System.Runtime.Caching.MemoryCache> 类是 <xref:System.Runtime.Caching.ObjectCache> 类的内存中对象缓存实现。 <xref:System.Runtime.Caching.MemoryCache> 类可用于大多数缓存任务。

> [!NOTE]
> <xref:System.Runtime.Caching.MemoryCache> 类在 <xref:System.Web.Caching> 命名空间中定义的 ASP.NET 缓存对象中建模。 因此，内部缓存逻辑与 ASP.NET 早期版本所提供的逻辑相似。

 有关如何在 WPF 应用程序中使用缓存的示例，请参阅[演练：在 WPF 应用程序中缓存应用程序数据](/dotnet/desktop/wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application)。

## <a name="caching-in-aspnet-applications"></a>在 ASP.NET 应用程序中进行缓存

 <xref:System.Runtime.Caching> 命名空间中的缓存类提供在 ASP.NET 中缓存数据的功能。

> [!NOTE]
> 如果你的应用程序以 .NET Framework 3.5 或更早版本为目标，则必须使用命名空间中定义的缓存类 <xref:System.Web.Caching> 。 有关详细信息，请参阅 [ASP.NET 缓存概述](/previous-versions/aspnet/ms178597(v=vs.100))。

> [!NOTE]
> 开发新应用程序时，建议使用 <xref:System.Runtime.Caching.MemoryCache> 类。 <xref:System.Runtime.Caching> 命名空间中提供的 API 与 <xref:System.Web.Caching.Cache> 命名空间中提供的 API 类似。 因此，如果你已在 ASP.NET 早期版本中使用过缓存，那么将会对 API 感到熟悉。 有关如何在 ASP.NET 应用程序中使用缓存的示例，请参阅[演练：在 ASP.NET 中缓存应用程序数据](/previous-versions/ff477235(v=vs.100))。

### <a name="output-caching"></a>输出缓存

 若要手动缓存应用程序数据，可使用 ASP.NET 中的 <xref:System.Runtime.Caching.MemoryCache> 类。 ASP.NET 还支持输出缓存，在内存中存储生成的页面输出、控件和 HTTP 响应。 可在 ASP.NET Web 页中通过声明方式或使用 Web.config 文件中的设置，配置输出缓存。 有关详细信息，请参阅[用于缓存的 outputCache 元素（ASP.NET 设置架构）](/previous-versions/dotnet/netframework-4.0/ms228124(v=vs.100))。

 借助 ASP.NET，可通过创建自定义输出缓存提供程序扩展输出缓存。 通过自定义提供程序，可使用磁盘、云存储和分布式缓存引擎等其他存储设备，存储缓存内容。 若要创建自定义输出缓存提供程序，可创建 <xref:System.Web.Caching.OutputCacheProvider> 类的派生类，并将应用程序配置为使用自定义输出缓存提供程序。

## <a name="caching-in-wcf-rest-services"></a>在 WCF REST 服务中进行缓存

 对于 WCF REST 服务，.NET Framework 可充分利用 ASP.NET 中提供的声明性输出缓存。 这便可以缓存来自 WCF REST 服务操作的响应。 如果用户向配置为进行缓存的服务发送了 HTTP GET 请求，那么 ASP.NET 将返回已缓存响应，且不会调用服务方法。 缓存过期后，下次用户发送 HTTP GET 请求时，将会调用服务方法且再次缓存响应。

 .NET Framework 还可实现条件 HTTP GET 缓存。 在 REST 方案中，条件 HTTP GET 请求通常由服务用来实现智能 HTTP 缓存，如 [HTTP 规范](https://www.w3.org/Protocols/rfc2616/rfc2616.html)中所述。 有关详细信息，请参阅 [Caching Support for WCF Web HTTP Services](../wcf/feature-details/caching-support-for-wcf-web-http-services.md)（对 WCF Web HTTP 服务的缓存支持）。

## <a name="extending-caching-in-the-net-framework"></a>在 .NET Framework 中扩展缓存

 .NET Framework 中的缓存设计为可扩展。 <xref:System.Runtime.Caching.ObjectCache> 类能够创建自定义缓存实现。 此类提供可用于所有托管应用程序（包括 Windows 窗体、Windows Presentation Foundation (WPF) 和 Windows Communications Foundation (WCF)）的成员。 若要创建使用不同存储机制的缓存类或希望精确地控制缓存操作，可能需要执行此操作。

 要扩展缓存可执行以下操作：

- 创建一个派生自 <xref:System.Runtime.Caching.ObjectCache> 类的自定义类，然后在此派生类中提供自定义缓存实现。

- 创建一个派生自 <xref:System.Runtime.Caching.MemoryCache> 类的类，并自定义或扩展此派生类。 有关如何执行此操作的示例，请参阅[在 ASP.NET 应用程序中使用多个缓存对象来缓存应用程序数据](/archive/blogs/aspnetue/caching-application-data-by-using-multiple-cache-objects-in-an-asp-net-application)。

- 创建 <xref:System.Web.Caching.OutputCacheProvider> 类的派生类，并配置应用程序以使用自定义输出缓存提供程序。

 有关详细信息，请参阅 Scott Guthrie 的博客上的进入 [可扩展输出缓存，其中包含 ASP.NET 4 (VS 2010 和 .NET Framework 4.0 系列) ](https://weblogs.asp.net/scottgu/extensible-output-caching-with-asp-net-4-vs-2010-and-net-4-0-series) 。

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Caching.ObjectCache>
- <xref:System.Runtime.Caching.MemoryCache>
- [演练：在 WPF 应用程序中缓存应用程序数据](/dotnet/desktop/wpf/advanced/walkthrough-caching-application-data-in-a-wpf-application)
- [演练：在 ASP.NET 中缓存应用程序数据](/previous-versions/ff477235(v=vs.100))
