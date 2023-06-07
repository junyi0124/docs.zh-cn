---
title: 107 -- BookmarkResumptionRecord
ms.date: 03/30/2017
ms.assetid: aa2d37ed-2bfa-439b-89e8-a9354027f155
ms.openlocfilehash: 14557ef2726a60bcde49d30f9d6168fd831e613c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238855"
---
# <a name="107----bookmarkresumptionrecord"></a>107 -- BookmarkResumptionRecord

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|107|  
|关键字|EndToEndMonitoring、Troubleshooting、HealthMonitoring、WFTracking|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 在工作流实例发出 BookmarkResumptionRecord 时，ETW 跟踪参与者将发出此事件。  
  
## <a name="message"></a>消息  

 TrackRecord = BookmarkResumptionRecord, InstanceID=%1, RecordNumber=%2,EventTime=%3, Name=%4, SubInstanceID=%5,  OwnerActivityName=%6, OwnerActivityId =%7, OwnerActivityInstanceId=%8, OwnerActivityTypeName=%9, Annotations=%10, ProfileName = %11  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InstanceId|xs:GUID|工作流的实例 ID|  
|RecordNumber|xs:long|发出的记录的序列号|  
|EventTime|xs:dateTime|发出该事件时的 UTC 时间|  
|“属性”|xs:string|继续的书签的名称|  
|SubInstanceID|xs:GUID|书签范围的 ID|  
|OwnerActivityName|xs:string|书签活动的名称|  
|OwnerActivityId|xs:string|书签活动的 ID|  
|OwnerActivityInstanceId|xs:string|书签活动的实例 ID|  
|OwnerActivityTypeName|xs:string|书签活动的类型|  
|批注|xs:string|已添加到此事件中的批注。  值存储在 xml 元素中，格式为 \<items> \< item  name = "annotationName" type="System.String"> a \</item> \</items> 。  如果未指定任何批注，则该字符串包含 \<items/> 。 ETW 事件大小受到 ETW 缓冲区大小或 ETW 事件最大负载的限制。 如果事件的大小超过 ETW 限制，则通过删除批注并将批注值替换为 ... 来截断事件。 \<items> \</items>|  
|ProfileName|xs:string|导致发出此事件的跟踪配置文件的名称|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。  其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName" 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
