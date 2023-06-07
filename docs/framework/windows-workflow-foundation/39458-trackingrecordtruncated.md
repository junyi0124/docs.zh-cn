---
title: 39458 - TrackingRecordTruncated
ms.date: 03/30/2017
ms.assetid: 5352f0eb-d571-454a-bab5-e2162888b218
ms.openlocfilehash: f02a34673c51be6e0b127a64e4622131575d836f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275887"
---
# <a name="39458---trackingrecordtruncated"></a>39458 - TrackingRecordTruncated

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|39458|  
|关键字|WFTracking|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示已截断跟踪记录。 已移除了变量/批注/用户数据。  
  
## <a name="message"></a>消息  

 用提供程序 %2 向 ETW 会话写入了截断的跟踪记录 %1。 已移除了变量/批注/用户数据  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|RecordNumber|xs:string|跟踪记录编号。|  
|ProviderId|xs:string|ETW 提供程序 ID。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
