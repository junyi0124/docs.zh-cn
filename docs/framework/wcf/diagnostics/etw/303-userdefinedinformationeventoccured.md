---
title: 303 - UserDefinedInformationEventOccured
ms.date: 03/30/2017
ms.assetid: 5ed5acaf-3755-4417-92c4-4ebc8e854ca1
ms.openlocfilehash: 8597d84184caea9fc5dc7778cfc6d05e7dc592db
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243425"
---
# <a name="303---userdefinedinformationeventoccured"></a>303 - UserDefinedInformationEventOccured

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|303|  
|关键字|疑难解答，HealthMonitoring，UserEvents，ServiceModel，EndToEndMonitoring|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 此事件从用户代码发出。 当开发人员的服务中出现自定义的信息性事件时，开发人员可以发出此事件。 可以使用 <xref:System.Diagnostics.Eventing> API 完成此操作。 此外，还提供了一个 WCF 示例，该示例包装 API 并演示如何正确发出此事件。  
  
## <a name="message"></a>消息  

 Name“%1”、Reference“%2”、Payload: %3  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|“属性”|`xs:string`|事件的用户定义名称|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|有效负载|`xs:string`|事件的用户定义负载。|
