---
title: 451 - MessageLogInfo
ms.date: 03/30/2017
ms.assetid: 485b4b29-dc21-4a35-93f8-5f4726d6aa5a
ms.openlocfilehash: 2b5dd36099e1d9978cb1136462c224a79465f823
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242781"
---
# <a name="451---messageloginfo"></a>451 - MessageLogInfo

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|451|  
|关键字|疑难解答，WCFMessageLogging|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 此事件是发送消息日志信息时发出的。  
  
## <a name="message"></a>消息  

 %1  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|data1|`xs:string`||  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
