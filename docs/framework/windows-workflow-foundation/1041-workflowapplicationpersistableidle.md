---
title: 1041 - WorkflowApplicationPersistableIdle
ms.date: 03/30/2017
ms.assetid: 966adf2f-e21d-44df-a3ec-a8e285e0a316
ms.openlocfilehash: 9995823753fc78ca3f278cd635fcf6c7d2b84325
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238933"
---
# <a name="1041---workflowapplicationpersistableidle"></a>1041 - WorkflowApplicationPersistableIdle

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1041|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示工作流应用程序空闲且可持久化。 工作流应用程序将是空闲且可持久化的。  
  
## <a name="message"></a>消息  

 WorkflowApplication Id "%1" 处于空闲和可持久状态。  将执行以下操作： %2。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|WorkflowApplicationId|xs:string|工作流应用程序 ID|  
|ActionTaken|xs:string|将对工作流应用程序执行的操作。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
