---
title: 1140 - FlowchartStart
ms.date: 03/30/2017
ms.assetid: 9aa2c71e-a4ab-4aed-b76d-4795e8493b70
ms.openlocfilehash: 7c3b22398926585177f4db9ffa5657d1fcb2bdde
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261795"
---
# <a name="1140---flowchartstart"></a>1140 - FlowchartStart

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1140|  
|关键字|WFActivities|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示已安排流程图启动。  
  
## <a name="message"></a>消息  

 Flowchart“%1”- 已安排启动。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|FlowChart|xs:string|FlowChart 的显示名称。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
