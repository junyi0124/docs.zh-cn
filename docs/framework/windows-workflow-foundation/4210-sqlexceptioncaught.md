---
title: 4210 - SqlExceptionCaught
ms.date: 03/30/2017
ms.assetid: f0ce8af3-eb4c-48c8-b3d9-dd2f94b5574b
ms.openlocfilehash: 1dab44e3fb97d74a2146f5bf992225222bc93d50
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280372"
---
# <a name="4210---sqlexceptioncaught"></a>4210 - SqlExceptionCaught

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4210|  
|关键字|WFInstanceStore|  
|Level|警告|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示捕获了 SQL 异常。  
  
## <a name="message"></a>消息  

 已捕获 SQL 异常编号为 %1 的消息 %2。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|ErrorNumber|xs:string|SQL 错误号。|  
|ExceptionMessage|xs:string|来自 SQL 异常的消息。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
