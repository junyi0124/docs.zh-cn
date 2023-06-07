---
title: Transactions Flowed Per Second（每秒流动的事务数）
ms.date: 03/30/2017
ms.assetid: b9f661e1-576c-48fc-9fdf-91853e0749e8
ms.openlocfilehash: b47219bd58a9b6c4401baa73177928ddca5b6a8b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254137"
---
# <a name="transactions-flowed-per-second"></a>Transactions Flowed Per Second（每秒流动的事务数）

计数器名称：Transactions Flowed Per Second（每秒流动的事务数）  
  
## <a name="description"></a>描述  

 一秒内向此操作流动的事务数量。 每当发送到此操作的消息中存在事务 ID 时，此计数器就会增加。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)
