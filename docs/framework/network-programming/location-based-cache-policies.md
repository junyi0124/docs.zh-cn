---
title: 基于位置的缓存策略
ms.date: 03/30/2017
helpviewer_keywords:
- Cache If Available policy
- reload policy
- location-based cache policies
- Cache Only policy
- local cache
- intermediate cache
- No Cache No Store policy
- cache [.NET Framework], location-based policies
- Revalidate policy
- freshness of cached resources
- Cache Or Next Cache Only policy
- Refresh policy
ms.assetid: e41d7f1a-0a6a-4dee-97d1-c6a8b6a07fc2
ms.openlocfilehash: 22d99111cd22ef24603f3cdd213c9c9afac5a974
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242196"
---
# <a name="location-based-cache-policies"></a>基于位置的缓存策略

基于位置的缓存策略根据可从中获取所请求资源的位置来定义有效缓存条目的新鲜度。 如果使用它不违反服务器指定的重新验证要求，则缓存资源为有效。 基于位置的缓存策略通过使用 <xref:System.Net.Cache.RequestCachePolicy> 或 <xref:System.Net.Cache.HttpRequestCachePolicy> 类构造函数以编程方式创建。 使用 <xref:System.Net.Cache.RequestCacheLevel> 或 <xref:System.Net.Cache.HttpRequestCacheLevel> 枚举值将基于位置的策略的类型传递给构造函数。 有关创建基于位置的缓存策略的代码示例，请参阅[如何：为应用程序设置基于位置的缓存策略](how-to-set-a-location-based-cache-policy-for-an-application.md)。 以下部分介绍超文本传输协议（http 和 https）资源每种基于位置的缓存策略。  
  
## <a name="cache-if-available-policy"></a>“如果可用则缓存”策略  

 如果本地缓存中存在有效的请求资源，则使用缓存资源；否则，将资源请求发送到服务器。 如果客户端和服务器之间的任何缓存中存在请求资源，则可由中间缓存满足该请求。  
  
## <a name="cache-only-policy"></a>“仅缓存”策略  

 如果本地缓存中存在有效的请求资源，则使用缓存资源。 指定此缓存策略级别时，如果本地缓存中没有该项，将引发 <xref:System.Net.WebException> 异常。  
  
## <a name="cache-or-next-cache-only-policy"></a>“缓存或仅下一个缓存”策略  

 如果有效的请求资源位于本地缓存或局域网的中间缓存，则使用缓存资源。 否则，将引发 <xref:System.Net.WebException> 异常。 在 HTTP 缓存协议中，这是通过 only-if-cached 缓存控制指令实现的。  
  
## <a name="no-cache-no-store-policy"></a>“无缓存无存储”策略  

 请求资源未从任何缓存中使用，并且未放于任何缓存中。 如果请求资源存在于本地缓存，则会将其删除。 此策略级别向中间缓存指示 - 它们也应删除资源。 在 HTTP 缓存协议中，这是通过 no-store 缓存控制指令实现的。  
  
## <a name="refresh-policy"></a>刷新策略  

 如果可从服务器获取请求资源，或可在本地缓存之外的缓存中找到请求资源，则可使用请求资源。 在请求可由中间缓存满足之前，该缓存必须通过服务器重新验证其缓存条目。 在 HTTP 缓存协议中，这是通过 max-age = 0 缓存控制指令和 no-cache Pragma 标头实现的。  
  
## <a name="reload-policy"></a>重新加载策略  

 请求资源必须从服务器获取。 响应可能保存在本地缓存。 在 HTTP 缓存协议中，这是通过 no-cache 缓存控制指令和 no-cache Pragma 标头实现的。  
  
## <a name="revalidate-policy"></a>重新验证策略  

 将缓存中的资源副本与服务器上的副本进行比较。 如果服务器上的副本较新，则用它来满足请求并替换缓存中的副本。 如果缓存中的副本与服务器副本相同，则使用缓存副本。 在 HTTP 缓存协议中，这是通过条件请求来实现的。  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于时间的缓存策略](time-based-cache-policies.md)
- [在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)
- [\<requestCaching> 元素（网络设置）](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
