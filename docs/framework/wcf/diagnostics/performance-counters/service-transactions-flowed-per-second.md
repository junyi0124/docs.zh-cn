---
title: 服务：Transactions Flowed Per Second（每秒流动的事务数）
ms.date: 03/30/2017
ms.assetid: ec72eb49-2942-4811-91df-d6e5dad81fd8
ms.openlocfilehash: 158bd7e2f2f98e91215ef7351cf90493a2d3059d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236827"
---
# <a name="service-transactions-flowed-per-second"></a>服务：Transactions Flowed Per Second（每秒流动的事务数）

计数器名称：Transactions Flowed Per Second（每秒流动的事务数）。  
  
## <a name="description"></a>描述  

 一秒内流向此服务中操作的事务数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
