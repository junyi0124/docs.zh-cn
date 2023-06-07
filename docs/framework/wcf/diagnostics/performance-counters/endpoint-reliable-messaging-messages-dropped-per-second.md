---
title: 终结点：Reliable Messaging Messages Dropped Per Second（每秒丢弃的可靠消息传递消息数）
ms.date: 03/30/2017
ms.assetid: ea3c4fc0-1e0f-4863-8879-57bc6c113018
ms.openlocfilehash: b8c0f91b2de5d023232756159a50236574308954
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290837"
---
# <a name="endpoint-reliable-messaging-messages-dropped-per-second"></a>终结点：Reliable Messaging Messages Dropped Per Second（每秒丢弃的可靠消息传递消息数）

计数器名称：Reliable Messaging Sessions Dropped Per Second（每秒丢弃的可靠消息会话数）。  
  
## <a name="description"></a>描述  

 此终结点上每秒钟丢弃的可靠传送消息总数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
