---
title: 终结点：Calls Failed Per Second（每秒失败的调用次数）
ms.date: 03/30/2017
ms.assetid: bcbe9da4-c8dd-4e27-b630-11611adc7580
ms.openlocfilehash: 0aeafa3d273ae22d9bbfe5e3edbac80c6e4851bd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96271248"
---
# <a name="endpoint-calls-failed-per-second"></a>终结点：Calls Failed Per Second（每秒失败的调用次数）

计数器名称：Calls Failed Per Second（每秒失败的调用次数）。  
  
## <a name="description"></a>描述  

 一秒钟内通过此终结点收到且具有未处理的异常的调用次数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)  
  
 每当此终结点处有未处理的异常时，此计数器就会增加。  
  
## <a name="see-also"></a>另请参阅

- [在协定和服务中指定和处理错误](../../specifying-and-handling-faults-in-contracts-and-services.md)
