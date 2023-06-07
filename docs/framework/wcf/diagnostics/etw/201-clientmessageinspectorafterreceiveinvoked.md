---
title: 201 - ClientMessageInspectorAfterReceiveInvoked
ms.date: 03/30/2017
ms.assetid: 9ff637f1-cc26-4400-ab9b-546f70e5057d
ms.openlocfilehash: d84f6c6f38868f7915caaaf87e15885099b65456
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266670"
---
# <a name="201---clientmessageinspectorafterreceiveinvoked"></a>201 - ClientMessageInspectorAfterReceiveInvoked

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|201|  
|关键字|疑难解答，ServiceModel|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 服务模型在客户端消息检查器上调用 `AfterReceiveReply` 方法之后，将发出此事件。  
  
## <a name="message"></a>消息  

 调度程序对“%1”类型的 ClientMessageInspector 调用了“AfterReceiveReply”。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|TypeName|`xs:string`|所调用检查器的类型的 CLR FullName。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
