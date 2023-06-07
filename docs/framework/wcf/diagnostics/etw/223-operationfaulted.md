---
title: 223 - OperationFaulted
ms.date: 03/30/2017
ms.assetid: 2f7d89d7-3a6a-40fe-9610-5424eb6bbf61
ms.openlocfilehash: 310e91320d27dd9817302fc14ef088d180152b73
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263069"
---
# <a name="223---operationfaulted"></a>223 - OperationFaulted

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|223|  
|关键字|EndToEndMonitoring、HealthMonitoring、疑难解答、ServiceModel|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 服务模型的默认 `OperationInvoker` 在调用其方法期间出现派生自 `FaultException` 的异常时，将发出此事件。  
  
## <a name="message"></a>消息  

 “%1”方法在由 OperationInvoker 调用时引发了一个 FaultException。 该方法调用持续了“%2”毫秒。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|MethodName|`xs:string`|由 `OperationInvoker` 调用的方法的 CLR 名称。|  
|持续时间|`xs:long`|`OperationInvoker` 调用方法所花费的时间（以毫秒为单位）。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
