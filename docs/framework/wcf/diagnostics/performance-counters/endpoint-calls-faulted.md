---
title: 终结点：Calls Faulted（出错的调用次数）
ms.date: 03/30/2017
ms.assetid: 271e6284-9c4b-465f-b619-069e1555a5e4
ms.openlocfilehash: da8f771e93a9457944046dfe84a1a7a455388d77
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250120"
---
# <a name="endpoint-calls-faulted"></a>终结点：Calls Faulted（出错的调用次数）

计数器名称：Calls Faulted（出错的调用次数）。  
  
## <a name="description"></a>描述  

 对此终结点返回错误的调用次数。 在 Windows Communication Foundation (WCF) 应用程序中，服务方法使用 SOAP 错误消息来传递处理错误信息。 SOAP 错误是包括在服务操作元数据中的消息类型，因此会创建一个错误协定，客户端可使用该协定来使执行更加可靠或更具交互性。 由于 SOAP 错误在客户端以 XML 格式表示，因此具有高度的互操作性。  
  
## <a name="see-also"></a>另请参阅

- [在协定和服务中指定和处理错误](../../specifying-and-handling-faults-in-contracts-and-services.md)
