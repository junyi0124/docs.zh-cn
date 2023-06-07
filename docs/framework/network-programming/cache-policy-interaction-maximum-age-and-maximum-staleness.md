---
title: 缓存策略交互 — 最长使用期限和最长过期时间
ms.date: 03/30/2017
helpviewer_keywords:
- maximum staleness
- freshness of cached resources
- time, cached resources
- maximum age policy
- staleness of cached resources
- age of cached resources
ms.assetid: 7f775925-89a1-4956-ba90-c869c1749a94
ms.openlocfilehash: bdfa608b5169755b2b4daaaa26e562308ae2be01
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250601"
---
# <a name="cache-policy-interactionmaximum-age-and-maximum-staleness"></a>缓存策略交互 — 最长使用期限和最长过期时间

为了帮助确保将最新鲜的内容返回给客户端应用程序，客户端缓存策略和服务器重新验证要求的交互始终会造成最保守的缓存策略。 本主题中的所有示例阐明针对在 1 月 1 日缓存、1 月 4 日过期的资源的缓存策略。  
  
 在以下示例中，结合使用了最长过期时间值 (`maxStale`) 与最长使用时间 (`maxAge`)：  
  
- 如果缓存策略设置 `maxAge` = 5 天，且未指定 `maxStale` 值，根据 `maxAge` 值，此内容在 1 月 6 日前可用。 但是，根据服务器的重新验证要求，内容会在 1 月 4 日过期。 因为内容过期日期更保守（更早），所以它优先于 `maxAge` 策略。 因此，即使尚未达到最长使用时间，此内容在 1 月 4 日便会过期，并且必须进行重新验证。  
  
- 如果缓存策略设置 `maxAge` = 5 天，`maxStale` = 3 天，根据 `maxAge` 值，此内容在 1 月 6 日前可用。 根据 `maxStale` 值，此内容在 1 月 7 日前可用。 因此，会在 1 月 6 日重新验证此内容。  
  
- 如果缓存策略设置 `maxAge` = 5 天，`maxStale` = 1 天，根据 `maxAge` 值，此内容在 1 月 6 日前可用。 根据 `maxStale` 值，此内容在 1 月 5 日前可用。 因此，会在 1 月 5 日重新验证此内容。  
  
 当内容的最长使用时间小于过期日期时，更保守的缓存行为占据优先级，且最长过期时间值不会有任何效果。 以下示例阐明了在内容过期之前到达最长使用时间 (`maxAge`) 时设置最长过期时间 (`maxStale`) 值的效果：  
  
- 如果缓存策略设置 `maxAge` = 1 天，且未指定 `maxStale` 值，即使内容尚未过期，也会在 1 月 2 日重新验证此内容。  
  
- 如果设缓存策略设置 `maxAge` = 1 天，`maxStale` = 3 天，会在 1 月 2 日重新验证此内容以强制实施更保守的策略设置。  
  
- 如果缓存策略设置 `maxAge` = 1 天，`maxStale` = 1 天，会在 1 月 2 日重新验证此内容。  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于位置的缓存策略](location-based-cache-policies.md)
- [基于时间的缓存策略](time-based-cache-policies.md)
- [在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)
- [缓存策略交互 — 最长使用时间和最低新鲜度](cache-policy-interaction-maximum-age-and-minimum-freshness.md)
