---
title: Calls Failed Per Second（每秒失败的调用次数）
ms.date: 03/30/2017
ms.assetid: e4ef3773-f650-4876-99cf-4d0c02aa03d4
ms.openlocfilehash: d3eafc4b31f0e62093a972b7c9f2325a3648d21b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285455"
---
# <a name="calls-failed-per-second"></a>Calls Failed Per Second（每秒失败的调用次数）

计数器名称：Calls Failed Per Second（每秒失败的调用次数）  
  
## <a name="description"></a>描述  

 该操作中每秒钟出现未处理异常的调用数目。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)  
  
 此操作中每次出现未处理的异常时，此计数器都会递增。  
  
## <a name="see-also"></a>另请参阅

- [在协定和服务中指定和处理错误](../../specifying-and-handling-faults-in-contracts-and-services.md)
