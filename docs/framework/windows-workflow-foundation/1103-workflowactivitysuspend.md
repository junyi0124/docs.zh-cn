---
title: 1103 - WorkflowActivitySuspend
ms.date: 03/30/2017
ms.assetid: b64e15c2-cb2c-4314-9074-ce2c6717232e
ms.openlocfilehash: 2fede703d086ed9653734f626fc38f56e073e416
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243568"
---
# <a name="1103---workflowactivitysuspend"></a>1103 - WorkflowActivitySuspend

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1103|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示工作流活动已挂起。  
  
## <a name="message"></a>消息  

 WorkflowInstance Id“%1”E2E 活动  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|WorkflowInstanceId|xs:string|工作流实例 ID。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
