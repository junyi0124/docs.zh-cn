---
title: 213 - ServiceHostStarted
ms.date: 03/30/2017
ms.assetid: a6f7facc-342f-46bb-9d52-a470178ba6bb
ms.openlocfilehash: 87a98d30f5ecde6f81580a95e337df22341c23d4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279020"
---
# <a name="213---servicehoststarted"></a>213 - ServiceHostStarted

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|213|  
|关键字|EndToEndMonitoring，HealthMonitoring，疑难解答，ServiceHost|  
|Level|LogAlways|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 在启动 Web 承载的服务主机时发出此事件。 这通常在消息激活服务时发生。 如果将服务配置为自动启动，也可能发生此事件。  
  
## <a name="message"></a>消息  

 ServiceHost 已启动:“%1”。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|服务类型名称|`xs:string`|服务实现的类型的 CLR FullName。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
