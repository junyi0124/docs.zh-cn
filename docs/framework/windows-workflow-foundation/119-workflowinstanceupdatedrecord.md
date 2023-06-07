---
title: 119 - WorkflowInstanceUpdatedRecord
ms.date: 03/30/2017
ms.assetid: 32485d0a-dcdb-45bc-b1e3-79fa9ad9439b
ms.openlocfilehash: c76ce2ffcd25ebe09463e6d704787f321baa2cb3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96278656"
---
# <a name="119---workflowinstanceupdatedrecord"></a>119 - WorkflowInstanceUpdatedRecord

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|119|  
|关键字|HealthMonitoring、WFTracking|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 当更新工作流实例时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord= WorkflowInstanceUpdatedRecord, InstanceID = %1, RecordNumber = %2, EventTime = %3, ActivityDefinitionId = %4, State = %5, OriginalDefinitionIdentity = %6, UpdatedDefinitionIdentity = %7, Annotations = %8, ProfileName = %9  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|ActivityDefinitionId|xs:string|工作流中根活动的名称|  
|状态|xs:string|工作流的当前状态。|  
|OriginalDefinitionIdentity|xs:string|原始工作流定义 ID|  
|UpdatedDefinitionIdentity|xs:string|已更新的工作流定义 ID|  
|批注|xs:string|已添加到此事件中的批注。 值存储在 xml 元素中，格式为 \<items> \< item name = "annotationName" type="System.String"> a \</item> \</items> 。 如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|WorkflowDefinitionIdentity|xs:string|工作流定义 ID|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
