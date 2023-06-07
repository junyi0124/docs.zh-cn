---
title: 108 - CustomTrackingRecordInfo
ms.date: 03/30/2017
ms.assetid: 5bee501e-4e00-42cd-b949-e88009c3d4e8
ms.openlocfilehash: 5fe45d62446d4dee23d29bed03dd490d8cb8efb8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238842"
---
# <a name="108---customtrackingrecordinfo"></a>108 - CustomTrackingRecordInfo

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|108|  
|关键字|UserEvents、EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 当工作流实例中的某个活动发出信息级别的 CustomTrackingRecord 时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord = CustomTrackingRecord, InstanceID = %1, RecordNumber=%2, EventTime=%3, Name=%4, ActivityName=%5, ActivityId=%6, ActivityInstanceId=%7, ActivityTypeName=%8, Data=%9, Annotations=%10, ProfileName = %11  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|“属性”|xs:string|CustomTrackingRecord 的名称|  
|ActivityName|xs:string|发出 CustomTrackingRecord 的活动的名称|  
|ActivityId|xs:string|发出 CustomTrackingRecord 的活动的 ID|  
|ActivityInstanceId|xs:string|发出 CustomTrackingRecord 的活动的实例 ID|  
|ActivityTypeName|xs:string|发出 CustomTrackingRecord 的活动的名称|  
|数据|xs:string|在此事件中跟踪的数据。  值存储在 xml 元素中，格式为 \<items> \< item  name = "dataName" type="System.String"> dataValue \</item> \</items> 。  如果未跟踪任何数据，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超出 ETW 限制，则通过删除批注并将数据值替换为 ... 来截断事件。 \<items> \</items> 以下类型作为其值存储，由 ToString ( # A1; 返回string、char、bool、int、short、long、uint、ushort、ulong、System.web、float、double、system.exception、system.exception、system.object。  所有其他类型使用 System.Runtime.Serialization.NetDataContractSerializer 进行序列化。|  
|批注|xs:string|已添加到此事件中的批注。  值存储在 xml 元素中，格式为 \<items> \< item  name = "annotationName" type="System.String"> a \</item> \</items> 。  如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。  其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName" 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
