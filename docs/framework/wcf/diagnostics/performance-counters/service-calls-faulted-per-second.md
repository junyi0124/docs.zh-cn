---
title: 服务：Calls Faulted Per Second（每秒出错的调用次数）
ms.date: 03/30/2017
ms.assetid: 94247356-2b29-4b50-b639-91ca8c1cf3a9
ms.openlocfilehash: d9db777d17de51caff74610d099a79228df1f6d8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252941"
---
# <a name="service-calls-faulted-per-second"></a>服务：Calls Faulted Per Second（每秒出错的调用次数）

计数器名称：Calls Faulted Per Second（每秒出错的调用次数）。  
  
## <a name="description"></a>描述  

 一秒内向此服务返回了错误的调用的数目。  
  
 此计数器的性能计数器类型 [PERF_COUNTER_COUNTER](/previous-versions/windows/it-pro/windows-server-2003/cc740048(v=ws.10))，其值使用以下公式进行计算。  
  
 (N 1 - N 0 ) / ( (D 1 -D 0 ) / F)  
  
 在 Windows Communication Foundation (WCF) 应用程序中，服务方法使用 SOAP 错误消息来传递处理错误信息。 SOAP 错误是包括在服务操作元数据中的消息类型，因此会创建一个错误协定，客户端可使用该协定来使执行更加可靠或更具交互性。 由于 SOAP 错误在客户端以 XML 格式表示，因此具有高度的互操作性。  
  
## <a name="see-also"></a>另请参阅

- [在协定和服务中指定和处理错误](../../specifying-and-handling-faults-in-contracts-and-services.md)
