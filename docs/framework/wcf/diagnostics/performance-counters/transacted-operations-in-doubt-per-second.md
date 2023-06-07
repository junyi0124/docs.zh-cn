---
title: Transacted Operations In Doubt Per Second（每秒不确定的事务处理操作次数）
ms.date: 03/30/2017
ms.assetid: 7e6b0716-c107-42e5-a21d-31d988e7a691
ms.openlocfilehash: 9162809ec467f282674e0ab21f7ce5e4d2222da4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249977"
---
# <a name="transacted-operations-in-doubt-per-second"></a>Transacted Operations In Doubt Per Second（每秒不确定的事务处理操作次数）

计数器名称：Transacted Operations In Doubt Per Second（每秒不确定的事务处理操作次数）。  
  
## <a name="description"></a>描述  

 此服务中每秒产生不确定结果的事务性操作的数量。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
