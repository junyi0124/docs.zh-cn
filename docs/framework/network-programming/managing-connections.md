---
title: 管理连接
description: 了解将 HTTP 用于数据资源的应用程序如何使用 .NET Framework ServicePoint 和 ServicePointManager 类管理连接。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Internet, connections
- HTTP, classes for connecting
- requesting data from Internet, connections
- sending data, connections
- receiving data, connections
- ServicePoint class, about ServicePoint class
- response to Internet request, connections
- connections [.NET Framework], classes
- network resources, connections
- downloading Internet resources, connections
- ServicePointManager class, about ServicePointManager class
ms.assetid: 9b3d3de7-189f-4f7d-81ae-9c29c441aaaa
ms.openlocfilehash: 00db05c99cf232a31b10bbd0356e6d43d3bc3e28
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282855"
---
# <a name="managing-connections"></a>管理连接

使用 HTTP 连接到数据资源的应用程序可使用 .NET Framework 的 <xref:System.Net.ServicePoint> 和 <xref:System.Net.ServicePointManager> 类管理与 Internet 的连接并有助于实现最优规模和最佳性能。  
  
 ServicePoint 类给应用程序提供了终结点，应用程序连接到终结点即可访问 Internet 资源。 每个 ServicePoint 都包含信息，该信息有助于通过共享连接之间的优化信息改进性能，优化与 Internet 服务器的连接。  
  
 每个 ServicePoint 都由统一资源标识符 (URI) 标识，并根据方案标识符和 URI 的主机片段分类。 例如，将由同一 ServicePoint 实例提供对 URI `http://www.contoso.com/index.htm` 和 `http://www.contoso.com/news.htm?date=today` 的请求，因为它们具有相同的方案标识符片段 (http) 和主机片段(`www.contoso.com`)。 如果应用程序已具有与服务器 `www.contoso.com` 的持久连接，它使用该连接检索两个请求，避免创建两个连接。  
  
 ServicePointManager 是管理 ServicePoint 实例的创建和析构的静态类。 如果应用程序请求一个不在现有  ServicePoint 实例集合里的 Internet 资源， ServicePointManager 将会创建 ServicePoint。 在 ServicePoint 实例超出最长空闲时间或现有数量超过应用程序的 ServicePoint 实例最大数量时，将销毁 ServicePoint 实例  。 可通过设置 ServicePointManager 上的 <xref:System.Net.ServicePointManager.MaxServicePointIdleTime%2A> 和 <xref:System.Net.ServicePointManager.MaxServicePoints%2A> 属性，来控制默认空闲时间的最大值和 ServicePoint 实例的最大数量。  
  
 客户端和服务器之间的连接数可能会对应用程序吞吐量产生重大影响。 默认情况下，使用 <xref:System.Net.HttpWebRequest> 类的应用程序最多使用与给定服务器的两个持久连接，但是，可以基于每个应用程序设置最大连接数。  
  
> [!NOTE]
> HTTP/1.1 规范将一个应用程序中的连接数限制为每个服务器两个连接。  
  
 连接的最佳数目取决于应用程序运行的实际情况。 增加应用程序的可用连接数可能不影响应用程序性能。 要确定更多连接的影响，请在改变连接数时运行性能测试。 在应用程序初始化时改变 ServicePointManager 类上的静态 <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A> 属性，可以更改应用程序使用的连接数，如以下代码示例所示。  
  
```csharp  
// Set the maximum number of connections per server to 4.  
ServicePointManager.DefaultConnectionLimit = 4;  
```  
  
```vb  
' Set the maximum number of connections per server to 4.  
ServicePointManager.DefaultConnectionLimit = 4  
```  
  
 更改  ServicePointManager.DefaultConnectionLimit 属性不会影响之前初始化的 ServicePoint 实例。 下面的代码演示如何将服务器 `http://www.contoso.com` 现有 ServicePoint 上的连接限制更改为存储在 `newLimit` 中的值。  
  
```csharp  
Uri uri = new Uri("http://www.contoso.com/");  
ServicePoint sp = ServicePointManager.FindServicePoint(uri);  
sp.ConnectionLimit = newLimit;  
```  
  
```vb  
Dim uri As New Uri("http://www.contoso.com/")  
Dim sp As ServicePoint = ServicePointManager.FindServicePoint(uri)  
sp.ConnectionLimit = newLimit  
```  
  
## <a name="see-also"></a>请参阅

- [连接分组](connection-grouping.md)
- [使用应用程序协议](using-application-protocols.md)
