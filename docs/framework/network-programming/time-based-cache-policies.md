---
title: 基于时间的缓存策略
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- cache synchronization date policy
- cache [.NET Framework], time-based policies
- freshness of cached resources
- time, cached resources
- maximum age policy
- synchronization, cache
- staleness of cached resources
- default time-based cache policy
- maximum staleness policy
- content cache policies
- expired content
- minimum freshness policy
- age of cached resources
ms.assetid: 74f0bcaf-5c95-40c1-9967-f3bbf1d2360a
ms.openlocfilehash: 372621ce55891cb87594e6d059c7bbeeb99f6468
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239414"
---
# <a name="time-based-cache-policies"></a>基于时间的缓存策略

基于时间的缓存策略使用检索资源的时间、随资源返回的标头和当前时间来定义缓存条目的新鲜度。 设置基于时间的缓存策略时，可使用 <xref:System.Net.Cache.HttpRequestCacheLevel.Default> 基于时间的策略，也可创建自定义的基于时间的策略。 当对使用超文本传输协议 (HTTP) 获得的资源使用默认的基于时间的策略时，由缓存响应中包含的标头以及 RFC 2616 第 13 和 14 节（可在 [Internet 工程任务组 (IETF)](https://www.ietf.org/) 网站上找到）中指定的行为来确定精确的缓存行为。 有关演示如何为 HTTP 资源设置默认的基于时间策略的代码示例，请参阅[如何：为应用程序设置默认基于时间的缓存策略](how-to-set-the-default-time-based-cache-policy-for-an-application.md)。 有关演示如何创建和使用缓存策略的代码示例，请参阅[在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)。  
  
## <a name="criteria-to-determine-freshness-of-cached-entries"></a>用于确定已缓存条目新鲜度的条件  

 要自定义基于时间的缓存策略，可指定使用以下一个或多个条件来确定已缓存条目的新鲜度：  
  
- 最长使用时间  
  
- 最长过期时间  
  
- 最低新鲜度  
  
- 缓存同步日期  
  
> [!NOTE]
> 使用默认的基于时间的缓存策略不应与设置应用程序的默认缓存策略混淆。 默认的基于时间的策略是可在请求级别或应用程序级别使用的特定策略。 应用程序的默认缓存策略是当未在请求中设置任何策略时生效的策略（基于位置或基于时间）。 有关为应用程序设置默认缓存策略的详细信息，请参阅 <xref:System.Net.WebRequest.DefaultCachePolicy%2A>。  
  
### <a name="maximum-age"></a>最长使用时间  

 最长使用时间策略条件指定资源的缓存副本可使用的时间。 如果资源的缓存副本超过指定的时间，则必须根据服务器上的内容进行检查，重新验证该资源。 如果最长使用时间允许在资源过期后使用资源，则不符合此条件，除非还指定了最长过期时间值。  
  
### <a name="maximum-staleness"></a>最长过期时间  

 最长过期时间策略条件指定资源的缓存副本内容过期后可使用的时间。 这是允许在资源过期后使用资源的唯一缓存策略条件。  
  
### <a name="minimum-freshness"></a>最低新鲜度  

 最低新鲜度策略条件指定资源的缓存副本内容过期前可使用的时间。 此策略的作用是使缓存条目在过期日之前过期，因此，最低新鲜度和最长过期时间设置是相互排斥的。  
  
## <a name="cache-synchronization-date"></a>缓存同步日期  

 缓存同步日期策略条件确定必须根据服务器上的内容进行检查以重新验证资源的缓存副本的时间。 如果缓存项目后内容发生更改，则将从服务器检索内容并存储在缓存中，然后返回到应用程序。 如果内容未更改，则会更新其时间戳，并且应用程序将获取缓存内容。  
  
 缓存同步日期允许指定必须重新验证缓存内容的绝对日期。 如果上一次重新验证新缓存条目是在缓存同步日期之前，则仍将根据服务器重新验证。 如果在缓存同步日期后重新验证缓存条目，且无其他新鲜度或服务器重新验证要求使缓存条目无效，则使用缓存中的条目。 如果缓存同步日期设置为未来某个日期，则每次请求时都会重新验证该条目，直到缓存同步日期过去。  
  
 以下主题介绍组合基于时间的缓存策略条件的影响：  
  
- [缓存策略交互 — 最长使用时间和最长过期时间](cache-policy-interaction-maximum-age-and-maximum-staleness.md)  
  
- [缓存策略交互 — 最长使用时间和最低新鲜度](cache-policy-interaction-maximum-age-and-minimum-freshness.md)  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于位置的缓存策略](location-based-cache-policies.md)
- [在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)
- [\<requestCaching> 元素（网络设置）](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
