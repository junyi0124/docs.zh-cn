---
title: 214 - OperationCompleted
ms.date: 03/30/2017
ms.assetid: a6287eef-023f-4816-813c-1802c82366df
ms.openlocfilehash: 6147c70448efb122cb43a2b42a1e9b59980fab29
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96278942"
---
# <a name="214---operationcompleted"></a>214 - OperationCompleted

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|214|  
|关键字|HealthMonitoring，EndToEndMonitoring，疑难解答，ServiceModel|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 如果服务模型的默认 `OperationInvoker` 已完成对某一方法的调用且未导致该方法引发异常，将发出此事件。  
  
## <a name="message"></a>消息  

 OperationInvoker 已完成对“%1”方法的调用。 该方法调用持续了“%2”毫秒。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|方法名|`xs:string`|由 `OperationInvoker` 调用的方法的 CLR 名称。|  
|持续时间|`xs:long`|`OperationInvoker` 调用方法所花费的时间（以毫秒为单位）。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
