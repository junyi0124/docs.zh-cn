---
title: 216 - MessageSentByTransport
ms.date: 03/30/2017
ms.assetid: 150c3167-4154-4225-8d94-57cc94341233
ms.openlocfilehash: b3e9e1a48951ad5a2e5e7820dc1828c20e310635
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96278903"
---
# <a name="216---messagesentbytransport"></a>216 - MessageSentByTransport

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|216|  
|关键字|疑难解答，ServiceModel|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 使用基于 TCP 的传输发送消息时发生此事件。 请注意，在传输级别上，单个操作可在客户端和服务之间交换多条消息。 这可能是由于基础结构行为的特点，在此方面，安全性是一个很好的例子。 因此，发出的 **MessageSentByTransport** 事件数因服务的绑定和其配置而异。  
  
## <a name="message"></a>消息  

 传输向“%1”发送了一条消息。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|目的地地址|`xs:string`|将请求消息发送到的地址。|  
|HostReference|xs:string|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。 其格式定义为 "网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;ServiceName"。 示例： "Default Web Site//Calculatorapplication&#124;/CalculatorService.svc&#124;CalculatorService"。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
