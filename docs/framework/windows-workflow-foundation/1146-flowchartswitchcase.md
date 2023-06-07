---
title: 1146 - FlowchartSwitchCase
ms.date: 03/30/2017
ms.assetid: 274e9209-1720-4512-a615-e742f00895f4
ms.openlocfilehash: 9636e5371440229ced965cf125ffb2ce4e314f72
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286105"
---
# <a name="1146---flowchartswitchcase"></a>1146 - FlowchartSwitchCase

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1146|  
|关键字|WFActivities|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示在 Flowchart 开关中已选择的大小写。  
  
## <a name="message"></a>消息  

 Flowchart“%1”/FlowSwitch - 选择了 Case“%2”。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|FlowChart|xs:string|FlowChart 的显示名称。|  
|案例|xs:string|所选的开关大小写。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
