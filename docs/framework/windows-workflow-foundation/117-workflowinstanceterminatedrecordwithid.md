---
title: 117 - WorkflowInstanceTerminatedRecordWithId
ms.date: 03/30/2017
ms.assetid: e68539d0-5338-468a-9f75-7e5b09d39a3c
ms.openlocfilehash: 5e16eff8e8ce9815cac604be110e899a29b4af3d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281724"
---
# <a name="117---workflowinstanceterminatedrecordwithid"></a>117 - WorkflowInstanceTerminatedRecordWithId

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|117|  
|关键字|HealthMonitoring、WFTracking|  
|Level|错误|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 当工作流实例发出 WorkflowInstanceTerminatedRecord 时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord = WorkflowInstanceTerminatedRecord, InstanceID = %1, RecordNumber = %2, EventTime = %3, ActivityDefinitionId = %4, Reason = %5, Annotations = %6, ProfileName = %7, WorkflowDefinitionIdentity = %8  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|ActivityDefinitionId|xs:string|工作流中根活动的名称|  
|状态|xs:string|工作流的当前状态。|  
|批注|xs:string|已添加到此事件中的批注。 值存储在 xml 元素中，格式为 \<items> \< item name = "annotationName" type="System.String"> a \</item> \</items> 。 如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|WorkflowDefinitionIdentity|xs:string|工作流定义 ID|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
