---
title: 1125 - InvokeMethodIsNotStatic
ms.date: 03/30/2017
ms.assetid: ea2b3827-63da-497b-b2c3-d5cebefe57a1
ms.openlocfilehash: 0405b4e1207db5c056fbd478b98c408258daf0c3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294204"
---
# <a name="1125---invokemethodisnotstatic"></a>1125 - InvokeMethodIsNotStatic

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1125|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 在 CacheMetadata 步骤中，InvokeMethod 活动指示要调用的方法是非静态的。  
  
## <a name="message"></a>消息  

 InvokeMethod“%1”- 方法非静态。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InvokeMethod|xs:string|InvokeMethod 活动的显示名称。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
