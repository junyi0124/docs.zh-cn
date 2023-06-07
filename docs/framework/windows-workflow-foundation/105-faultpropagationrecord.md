---
title: 105 - FaultPropagationRecord
ms.date: 03/30/2017
ms.assetid: 168473b1-b1e5-4e9f-8a2a-35bbdb2ef531
ms.openlocfilehash: 3390a77f16cc52e52ea1b3e4c1a34d0f44795abb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238907"
---
# <a name="105---faultpropagationrecord"></a>105 - FaultPropagationRecord

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|105|  
|关键字|EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 当工作流实例中的某个活动发出 FaultPropagationRecord 时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord = FaultPropagationRecord, InstanceID=%1, RecordNumber=%2, EventTime=%3, FaultSourceActivityName=%4, FaultSourceActivityId=%5, FaultSourceActivityInstanceId=%6, FaultSourceActivityTypeName=%7, FaultHandlerActivityName=%8, FaultHandlerActivityId = %9, FaultHandlerActivityInstanceId =%10, FaultHandlerActivityTypeName=%11, Fault=%12, IsFaultSource=%13, Annotations=%14, ProfileName = %15  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|FaultSourceActivityName|xs:string|发出错误的活动的名称|  
|FaultSourceActivityId|xs:string|发出错误的活动的 ID|  
|FaultSourceActivityInstanceId|xs:string|发出错误的活动的实例 ID|  
|FaultSourceActivityTypeName|xs:string|发出错误的活动的类型|  
|FaultHandlerActivityName|xs:string|错误处理程序活动的显示名称|  
|FaultHandlerActivityId|xs:string|错误处理程序活动的 ID|  
|FaultHandlerActivityInstanceId|xs:string|错误处理程序活动的实例 ID|  
|FaultHandlerActivityTypeName|xs:string|错误处理程序活动的类型|  
|故障|xs:string|错误详细信息|  
|IsFaultSource|xs:unsignedByte|指示事件是否从出错源发出|  
|批注|xs:string|已添加到此事件中的批注。  值存储在 xml 元素中，格式为 \<items> \< item  name = "annotationName" type="System.String"> a \</item> \</items> 。  如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。  其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName" 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
