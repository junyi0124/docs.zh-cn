---
title: 每秒实例数
ms.date: 03/30/2017
ms.assetid: 74579397-1058-4278-80cf-2d00854a480f
ms.openlocfilehash: ac535de1e8dacb97c136d72d096631d838d26700
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266085"
---
# <a name="instances-per-second"></a>每秒实例数

计数器名称：Instances Created Per Second（每秒创建的实例数）。  
  
## <a name="description"></a>描述  

 每秒创建的服务实例的总数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
