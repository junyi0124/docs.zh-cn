---
title: 4211 - QueuingSqlRetry
ms.date: 03/30/2017
ms.assetid: df569f88-c86b-4503-840d-1399b67f4df7
ms.openlocfilehash: ff8a1e099934f5bf71fef0afbb7e54c0d1851fae
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267177"
---
# <a name="4211---queuingsqlretry"></a>4211 - QueuingSqlRetry

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4211|  
|关键字|WFInstanceStore|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示将 SQL 重试排队。  
  
## <a name="message"></a>消息  

 正在将 SQL 重试排队，延迟 %1 毫秒。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|延迟|xs:string|重试之间的延迟。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
