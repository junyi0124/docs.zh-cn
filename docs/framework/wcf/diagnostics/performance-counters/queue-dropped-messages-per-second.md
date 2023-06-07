---
title: 每秒丢弃的排队消息数
ms.date: 03/30/2017
ms.assetid: 74540f52-8762-4147-b5ba-e171180515a3
ms.openlocfilehash: 81ba15965676ba7ffe5ca2aaac5e1d0e94e27962
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266033"
---
# <a name="queue-dropped-messages-per-second"></a>每秒丢弃的排队消息数

计数器名称：Queued Messages Dropped Per Second（每秒丢弃的排队消息数）。  
  
## <a name="description"></a>描述  

 此服务处的排队传输机制每秒丢弃的消息数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
