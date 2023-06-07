---
title: 1005 - WorkflowApplicationIdled
ms.date: 03/30/2017
ms.assetid: 74d77dfa-f20d-4fe9-a6ae-e6d1b5fe4182
ms.openlocfilehash: 3b7210246b7fb754145c8aa6128da3183cea9f91
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239856"
---
# <a name="1005---workflowapplicationidled"></a>1005 - WorkflowApplicationIdled

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1005|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示工作流应用程序已处于空闲状态。  
  
## <a name="message"></a>消息  

 WorkflowApplication ID“%1”变为空闲状态。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|WorkflowInstanceId|`xs:string`|工作流应用程序 ID|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
