---
title: 网络应用程序的缓存管理
ms.date: 03/30/2017
helpviewer_keywords:
- cache [.NET Framework], network applications
- network resources, caching
- Internet, caching
ms.assetid: fc258a40-f370-434f-ae09-4a8cb11ddaeb
ms.openlocfilehash: 81f0eaa33b185c6bfbc8758e73a68a6bfc248872
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287561"
---
# <a name="cache-management-for-network-applications"></a>网络应用程序的缓存管理

本主题及其相关的副主题描述针对使用 <xref:System.Net.WebClient>、<xref:System.Net.WebRequest>、<xref:System.Net.HttpWebRequest> 和 <xref:System.Net.FtpWebRequest> 类获取的资源的缓存。  
  
 缓存为已由应用程序请求的资源提供临时存储。 如果应用程序多次请求同一资源，则可以从此缓存中返回此资源，避免了重复向服务器发出请求的开销。 缓存可以通过减少获取请求的资源所需的时间，以提高应用程序性能。 缓存还可以通过减少对服务器的访问次数，以减少网络流量。 虽然缓存提高了性能，但会增加返回到应用程序的资源已过时的风险，这意味着如果未使用缓存，返回到应用程序的资源则与由服务器发送的资源不同。  
  
 缓存可以允许未经授权的用户或进程读取敏感数据。 在无需进行额外授权的情况下，可以从缓存中检索缓存的已经过身份验证的响应。 如果已启用缓存，请将 <xref:System.Net.WebRequest.CachePolicy%2A> 更改为 <xref:System.Net.Cache.RequestCacheLevel.BypassCache> 或 <xref:System.Net.Cache.RequestCacheLevel.NoCacheNoStore> 以针对此请求禁用缓存。  
  
 出于安全考虑，不建议将缓存用于中间层方案  。  
  
## <a name="in-this-section"></a>本节内容  

 [缓存策略](cache-policy.md)  
 说明什么是缓存策略以及如何定义一个缓存策略。  
  
 [基于位置的缓存策略](location-based-cache-policies.md)  
 定义基于位置的缓存策略的每个类型，此策略可用于超文本传输协议（http 和 https）资源。  
  
 [基于时间的缓存策略](time-based-cache-policies.md)  
 描述可用于自定义基于时间的缓存策略的条件。  
  
 [在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)  
 描述如何以编程方式创建缓存策略以及使用缓存的请求。  
  
## <a name="reference"></a>引用  

 <xref:System.Net.Cache>  
 定义类型和枚举，这些类型和枚举用于为使用 <xref:System.Net.WebRequest><xref:System.Net.HttpWebRequest> 和 <xref:System.Net.FtpWebRequest> 类获取的资源定义缓存策略。
