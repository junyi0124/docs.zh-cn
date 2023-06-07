---
title: 缓存策略交互 — 最长使用期限和最低新鲜度
ms.date: 03/30/2017
helpviewer_keywords:
- time-based cache policies
- Revalidate policy
- cache [.NET Framework], time-based policies
- freshness of cached resources
- maximum age policy
- minimum freshness policy
- age of cached resources
ms.assetid: 6567d451-ecec-496c-95a3-a415b99ba52a
ms.openlocfilehash: d4182268341f4a4334a627fc8c9e24fa235f003f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287548"
---
# <a name="cache-policy-interactionmaximum-age-and-minimum-freshness"></a>缓存策略交互 — 最长使用期限和最低新鲜度

为了帮助确保将最新鲜的内容返回给客户端应用程序，客户端缓存策略和服务器重新验证要求的交互始终会造成最保守的缓存策略。 本主题中的所有示例阐明针对在 1 月 1 日缓存、1 月 4 日过期的资源的缓存策略。  
  
 以下示例将说明最长使用时间值 (`maxAge`) 和最低新鲜度值 (`minFresh`) 交互产生的缓存策略。  
  
- 如果缓存策略设置 `maxAge` = 2 天，`minFresh` 未指定，则会在 1 月 3 日重新验证此内容。  
  
- 如果缓存策略设置 `maxAge` = 2 天，`minFresh` = 1 天，根据 `maxAge`，内容在 1 月 3 日前是新鲜的。 根据 `minFresh`，此内容在 1 月 3 日前也是新鲜的。 因此，必须在 1 月 3 日重新验证此内容。  
  
- 如果缓存策略设置 `maxAge` = 2 天，`minFresh` = 2 天，根据 `maxAge`，此内容在 1 月 3 日前是新鲜的。 根据 `minFresh`，此内容在 1 月 2 日前也是新鲜的。 因此，必须在 1 月 2 日重新验证此内容。  
  
## <a name="see-also"></a>另请参阅

- [网络应用程序的缓存管理](cache-management-for-network-applications.md)
- [缓存策略](cache-policy.md)
- [基于位置的缓存策略](location-based-cache-policies.md)
- [基于时间的缓存策略](time-based-cache-policies.md)
- [在网络应用程序中配置缓存](configuring-caching-in-network-applications.md)
- [缓存策略交互 — 最长使用时间和最长过期时间](cache-policy-interaction-maximum-age-and-maximum-staleness.md)
