---
title: 如何：自定义基于时间的缓存策略
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time-based cache policies
- customizing time-based cache policies
- cache [.NET Framework], time-based policies
ms.assetid: 8d84f936-2376-4356-9264-03162e0f9279
ms.openlocfilehash: 1a2ba404e333eeec2a23758c834876d0df5aba81
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "73040637"
---
# <a name="how-to-customize-a-time-based-cache-policy"></a>如何：自定义基于时间的缓存策略

创建基于时间的缓存策略时，可以通过为最长使用时间、最低新鲜度、最长过期时间或缓存同步日期指定值，以自定义缓存行为。 <xref:System.Net.Cache.HttpRequestCachePolicy> 对象提供几个构造函数，可用于指定这些值的有效组合。

## <a name="to-create-a-time-based-cache-policy-that-uses-a-cache-synchronization-date"></a>创建使用缓存同步日期的基于时间的缓存策略

通过将 <xref:System.DateTime> 对象传递给 <xref:System.Net.Cache.HttpRequestCachePolicy> 构造函数，可创建使用缓存同步日期的基于时间的缓存策略：

```csharp
public static HttpRequestCachePolicy CreateLastSyncPolicy(DateTime when)
{
    var policy = new HttpRequestCachePolicy(when);
    Console.WriteLine("When: {0}", when);
    Console.WriteLine(policy.ToString());
    return policy;
}
```

```vb
Public Shared Function CreateLastSyncPolicy([when] As DateTime) As HttpRequestCachePolicy
    Dim policy As New HttpRequestCachePolicy([when])
    Console.WriteLine("When: {0}", [when])
    Console.WriteLine(policy.ToString())
    Return policy
End Function
```

此输出类似于以下内容：

```output
When: 1/14/2004 8:07:30 AM
Level:Default CacheSyncDate:1/14/2004 8:07:30 AM
```

## <a name="to-create-a-time-based-cache-policy-that-is-based-on-minimum-freshness"></a>创建基于最低新鲜度的基于时间的缓存策略

通过将 <xref:System.Net.Cache.HttpCacheAgeControl.MinFresh> 指定为 `cacheAgeControl` 参数值，并且将 <xref:System.TimeSpan> 对象传递给 <xref:System.Net.Cache.HttpRequestCachePolicy> 构造函数，可创建基于最低新鲜度的基于时间的缓存策略：

```csharp
public static HttpRequestCachePolicy CreateMinFreshPolicy(TimeSpan span)
{
    var policy = new HttpRequestCachePolicy(HttpCacheAgeControl.MinFresh, span);
    Console.WriteLine(policy.ToString());
    return policy;
}
```

```vb
Public Shared Function CreateMinFreshPolicy(span As TimeSpan) As HttpRequestCachePolicy
    Dim policy As New HttpRequestCachePolicy(HttpCacheAgeControl.MinFresh, span)
    Console.WriteLine(policy.ToString())
    Return policy
End Function
```

对于以下调用：

```csharp
CreateMinFreshPolicy(new TimeSpan(1,0,0));
```

输出为：

```output
Level:Default MinFresh:3600
```

## <a name="to-create-a-time-based-cache-policy-that-is-based-on-minimum-freshness-and-maximum-age"></a>创建基于最低新鲜度和最长使用时间的基于时间的缓存策略

通过将 <xref:System.Net.Cache.HttpCacheAgeControl.MaxAgeAndMinFresh> 指定为 `cacheAgeControl` 参数值，并且将两个 <xref:System.TimeSpan> 对象（一个用于指定资源的最长使用时间，另一个用于指定缓存中返回对象的最低新鲜度）传递到 <xref:System.Net.Cache.HttpRequestCachePolicy> 构造函数，可创建基于最低新鲜度和最长使用时间的一个基于时间的缓存策略：

```csharp
public static HttpRequestCachePolicy CreateFreshAndAgePolicy(TimeSpan freshMinimum, TimeSpan ageMaximum)
{
    var policy = new HttpRequestCachePolicy(HttpCacheAgeControl.MaxAgeAndMinFresh, ageMaximum, freshMinimum);
    Console.WriteLine(policy.ToString());
    return policy;
}
```

```vb
Public Shared Function CreateFreshAndAgePolicy(freshMinimum As TimeSpan, ageMaximum As TimeSpan) As HttpRequestCachePolicy
    Dim policy As New HttpRequestCachePolicy(HttpCacheAgeControl.MaxAgeAndMinFresh, ageMaximum, freshMinimum)
    Console.WriteLine(policy.ToString())
    Return policy
End Function
```

对于以下调用：
  
```csharp
CreateFreshAndAgePolicy(new TimeSpan(5,0,0), new TimeSpan(10,0,0));  
```  

输出为：
  
```output
Level:Default MaxAge:36000 MinFresh:18000  
```  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于位置的缓存策略](location-based-cache-policies.md)
- [基于时间的缓存策略](time-based-cache-policies.md)
- [\<requestCaching> 元素（网络设置）](../configure-apps/file-schema/network/requestcaching-element-network-settings.md)
