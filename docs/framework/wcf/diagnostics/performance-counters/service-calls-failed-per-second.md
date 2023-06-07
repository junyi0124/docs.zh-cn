---
title: 服务：Calls Failed Per Second（每秒失败的调用次数）
ms.date: 03/30/2017
ms.assetid: 5a2c7939-107d-4f0c-b43c-e02e079e8a9d
ms.openlocfilehash: c97e4b0c6c2c71756a9bed7b1a2359ad0c118a98
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252967"
---
# <a name="service-calls-failed-per-second"></a>服务：Calls Failed Per Second（每秒失败的调用次数）

计数器名称：Calls Failed Per Second（每秒失败的调用次数）。  
  
## <a name="description"></a>描述  

 一秒钟内由此服务收到且具有未处理异常的调用次数。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)  
  
 在托管代码中，出现错误条件时引发异常。  
  
 在托管代码中，出现错误条件时引发异常。  
  
 此服务中每出现一个未处理异常，此计数器就会递增。  
  
## <a name="see-also"></a>另请参阅

- [在协定和服务中指定和处理错误](../../specifying-and-handling-faults-in-contracts-and-services.md)
