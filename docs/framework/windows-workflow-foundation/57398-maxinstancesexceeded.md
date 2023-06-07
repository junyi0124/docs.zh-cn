---
title: 57398 - MaxInstancesExceeded
ms.date: 03/30/2017
ms.assetid: f943d209-dfeb-43e5-b572-c9a06217936e
ms.openlocfilehash: bd490aad24fba4550bc778799cd6f836dcfd466c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289173"
---
# <a name="57398---maxinstancesexceeded"></a>57398 - MaxInstancesExceeded

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|57398|  
|关键字|WFServices|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 指示系统达到为限制“MaxConcurrentInstances”设置的限制值。  
  
## <a name="message"></a>消息  

 系统达到为限制“MaxConcurrentInstances”设置的限制值。 此限制的限制值设置为 %1。 可通过修改 serviceThrottle 元素中的特性“maxConcurrentInstances”或修改行为 ServiceThrottlingBehavior 的“MaxConcurrentInstances”属性来更改限制值。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|“属性”|xs:string|项的名称。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
