---
title: 服务：Calls Per Second（每秒调用次数）
ms.date: 03/30/2017
ms.assetid: 6261d28d-d449-425a-b9fc-a4ee14079134
ms.openlocfilehash: 7e702d402909c4a85a2cb42c837e0813c4d9f841
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252876"
---
# <a name="service-calls-per-second"></a>服务：Calls Per Second（每秒调用次数）

计数器名称：Calls Per Second（每秒调用次数）  
  
## <a name="description"></a>描述  

 一秒内调用此服务的次数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)。其中，分子 (N) 代表上一个取样时间间隔内执行的操作数，而分母 (D) 代表上一个取样时间间隔内所用的滴答数，而 F 则是滴答频率。
