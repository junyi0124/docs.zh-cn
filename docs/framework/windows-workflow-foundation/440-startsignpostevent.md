---
title: 440 - StartSignpostEvent1
ms.date: 03/30/2017
ms.assetid: 27b551b5-ae76-49f8-bab8-6300009eb4c1
ms.openlocfilehash: 1e0278d665a961afab21445ab8490e3e5a94987c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293450"
---
# <a name="440---startsignpostevent1"></a>440 - StartSignpostEvent1

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|440|  
|关键字|疑难解答|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 在活动跟踪中，指示消息是否已开始跨越发送或接收中的活动边界。  
  
## <a name="message"></a>消息  

 活动边界。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|ExtendedData|`xs:string`|活动的名称。|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
