---
title: 如何：为应用程序设置基于位置的缓存策略
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- explicitly defining cache behavior
- location-based cache policies
- local cache
- request cache policies
- cache [.NET Framework], location-based policies
ms.assetid: 683bb88e-3411-4f46-9686-3411b6ba511c
ms.openlocfilehash: 7331845c391265d72d3025fd9bf7856d83c783e9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253487"
---
# <a name="how-to-set-a-location-based-cache-policy-for-an-application"></a>如何：为应用程序设置基于位置的缓存策略

基于位置的缓存策略允许应用程序基于所请求资源的位置显式定义缓存行为。 本主题演示如何以编程方式设置缓存策略。 有关使用配置文件为应用程序设置策略的信息，请参阅 [\<requestCaching> 元素（网络设置）](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)。  
  
### <a name="to-set-a-location-based-cache-policy-for-an-application"></a>为应用程序设置基于位置的缓存策略  
  
1. 创建 <xref:System.Net.Cache.RequestCachePolicy> 或 <xref:System.Net.Cache.HttpRequestCachePolicy> 对象。  
  
2. 将策略对象设置为应用程序域的默认对象。  
  
### <a name="to-set-a-policy-that-takes-requested-resources-from-a-cache"></a>设置获取来自缓存的请求资源的策略  
  
- 创建获取来自缓存的请求资源的策略（若可用），否则通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.CacheIfAvailable> 向服务器发送请求。 可以由客户端和服务器之间的任何缓存实现请求，包括远程缓存。  
  
    ```csharp  
    public static void UseCacheIfAvailable()  
    {  
        HttpRequestCachePolicy policy = new HttpRequestCachePolicy  
            (HttpRequestCacheLevel.CacheIfAvailable);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub UseCacheIfAvailable()  
        Dim policy As New HttpRequestCachePolicy _  
             (HttpRequestCacheLevel.CacheIfAvailable)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-any-cache-from-supplying-resources"></a>设置防止任何缓存提供资源的策略  
  
- 通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.NoCacheNoStore>，创建防止缓存提供请求的资源的策略。 此策略级别从本地缓存中删除资源（如果存在），并指示远程缓存也应删除资源。  
  
    ```csharp  
    public static void DoNotUseCache()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.NoCacheNoStore);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub DoNotUseCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.NoCacheNoStore)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-returns-requested-resources-only-if-they-are-in-the-local-cache"></a>设置仅当请求的资源在本地缓存中时返回请求的资源的策略  
  
- 通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.CacheOnly>，创建仅当请求的资源在本地缓存中时返回请求的资源的策略。 如果请求的资源不在缓存中，将引发 <xref:System.Net.WebException> 异常。  
  
    ```csharp  
    public static void OnlyUseCache()  
    {  
        HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.CacheOnly);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub OnlyUseCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.CacheOnly)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-the-local-cache-from-supplying-resources"></a>设置防止本地缓存提供资源的策略  
  
- 通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.Refresh>，创建防止本地缓存提供请求的资源的策略。 如果请求的资源是中间缓存并已成功重新验证，中间缓存可提供请求的资源。  
  
    ```csharp  
    public static void DoNotUseLocalCache()  
    {  
     HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.Refresh);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub DoNotUseLocalCache()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Refresh)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-prevents-any-cache-from-supplying-requested-resources"></a>设置防止任何缓存提供请求的资源的策略  
  
- 通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.Reload>，创建防止缓存提供请求的资源的策略。 服务器返回的资源可以存储在缓存中。  
  
    ```csharp  
    public static void SendToServer()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy
            (HttpRequestCacheLevel.Reload);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub SendToServer()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Reload)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
### <a name="to-set-a-policy-that-allows-any-cache-to-supply-requested-resources-if-the-resource-on-the-server-is-not-newer-than-the-cached-copy"></a>设置当服务器上的资源不比缓存副本新时，允许任何缓存提供请求的资源的策略  
  
- 通过将缓存级别设置为 <xref:System.Net.Cache.HttpRequestCacheLevel.Revalidate>，创建当服务器上的资源不比缓存副本新时，允许任何缓存提供请求的资源的策略。  
  
    ```csharp  
    public static void CheckServer()  
    {  
    HttpRequestCachePolicy policy = new HttpRequestCachePolicy  
             (HttpRequestCacheLevel.Revalidate);  
        HttpWebRequest.DefaultCachePolicy = policy;  
    }  
    ```  
  
    ```vb  
    Public Shared Sub CheckServer()  
        Dim policy As New HttpRequestCachePolicy _  
            (HttpRequestCacheLevel.Revalidate)  
        HttpWebRequest.DefaultCachePolicy = policy  
    End Sub  
    ```  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于位置的缓存策略](location-based-cache-policies.md)
- [基于时间的缓存策略](time-based-cache-policies.md)
- [\<requestCaching> 元素（网络设置）](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
