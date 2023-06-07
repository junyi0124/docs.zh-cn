---
title: 100 - WorkflowInstanceRecord
ms.date: 03/30/2017
ms.assetid: ed4d1851-b378-489b-a22d-c1db09571fb4
ms.openlocfilehash: c0780aa21e76eb0d72f83b5d2de7d16ff84a6ac7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240038"
---
# <a name="100---workflowinstancerecord"></a>100 - WorkflowInstanceRecord

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|100|  
|关键字|EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 工作流实例发出以下工作流状态的 WorkflowInstanceRecord 时，ETW 跟踪参与者将发出此事件：已启动、已继续、已持久保存、空闲、已删除、已完成、已取消、已卸载和已取消挂起。  
  
## <a name="message"></a>消息  

 TrackRecord= WorkflowInstanceRecord, InstanceID = %1, RecordNumber = %2, EventTime = %3, ActivityDefinitionId = %4, State = %5, Annotations = %6, ProfileName = %7  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|ActivityDefinitionId|xs:string|工作流中根活动的名称|  
|状态|xs:string|工作流的当前状态。|  
|批注|xs:string|已添加到此事件中的批注。  值存储在 xml 元素中，格式为 \<items> \< item  name = "annotationName" type="System.String"> a \</item> \</items> 。  如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。  其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName" 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
