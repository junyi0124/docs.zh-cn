---
title: 103 - ActivityStateRecord
ms.date: 03/30/2017
ms.assetid: 57636a9a-561e-44aa-aef9-1f1894aaa6dd
ms.openlocfilehash: 02c33f02b7650c9f9b7527c319de3b58980fdd6c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275071"
---
# <a name="103---activitystaterecord"></a>103 - ActivityStateRecord

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|103|  
|关键字|EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 当工作流实例中的某个活动发出 ActivityStateRecord 时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord = ActivityStateRecord, InstanceID = %1, RecordNumber=%2, EventTime=%3, State = %4, Name=%5, ActivityId=%6, ActivityInstanceId=%7, ActivityTypeName=%8, Arguments=%9, Variables=%10, Annotations=%11, ProfileName = %12  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|状态|xs:string|活动的状态|  
|“属性”|xs:string|发出该事件的活动的显示名称|  
|ActivityId|xs:string|发出的活动的活动 ID|  
|ActivityInstanceId|xs:string|发出的活动的活动实例 ID|  
|ActivityTypeName|xs:string|发出的活动的类型名称|  
|参数|xs:string|在此事件中跟踪的参数。  值存储在 xml 元素中，格式为 \<items> \< item  name = "argumentName" type="System.String"> argumentValue \</item> \</items> 。  如果未跟踪任何参数，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items> 以下类型作为其值存储，由 ToString ( # A1; 返回string、char、bool、int、short、long、uint、ushort、ulong、System.web、float、double、system.exception、system.exception、system.object。  所有其他类型使用 System.Runtime.Serialization.NetDataContractSerializer 进行序列化。|  
|变量|xs:string|在此事件中跟踪的变量。  值存储在 xml 元素中，格式为 \<items> \< item  name = "variableName" type="System.String"> variableValue \</item> \</items> 。  如果未跟踪任何变量，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将变量值替换为 ... 来截断事件。 \<items> \</items> 以下类型作为其值存储，由 ToString ( # A1; 返回string、char、bool、int、short、long、uint、ushort、ulong、System.web、float、double、system.exception、system.exception、system.object。  所有其他类型使用 System.Runtime.Serialization.NetDataContractSerializer 进行序列化。|  
|批注|xs:string|已添加到此事件中的批注。  值存储在 xml 元素中，格式为 \<items> \< item  name = "annotationName" type="System.String"> a \</item> \</items> 。  如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。  其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName" 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
