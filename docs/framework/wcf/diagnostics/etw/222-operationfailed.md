---
title: 222 - OperationFailed
ms.date: 03/30/2017
ms.assetid: 6b530ded-8f20-4d78-8bfe-1875276df6ba
ms.openlocfilehash: 64b41ee78e943ca16eaa791133454ec62ccf6ed8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263082"
---
# <a name="222---operationfailed"></a>222 - OperationFailed

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|222|  
|关键字|EndToEndMonitoring、HealthMonitoring、疑难解答、ServiceModel|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 如果服务模型的默认 `OperationInvoker` 在调用其方法时遇到异常，则会发出此事件。 请注意，派生自 `FaultException` 的异常会导致不发出此事件。  
  
## <a name="message"></a>消息  

 “%1”方法在由 OperationInvoker 调用时引发了未经处理的异常。 该方法调用持续了“%2”毫秒。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|方法名|`xs:string`|由 `OperationInvoker` 调用的方法的 CLR 名称。|  
|持续时间|`xs:long`|`OperationInvoker` 调用方法所花费的时间（以毫秒为单位）。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
