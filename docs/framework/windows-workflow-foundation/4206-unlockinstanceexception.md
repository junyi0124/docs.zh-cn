---
title: 4206 - UnlockInstanceException
ms.date: 03/30/2017
ms.assetid: 5a46dc5f-d517-4135-8905-25a42f01206b
ms.openlocfilehash: 48182d7c5fe8f29842a17f28c0ea296f93b31089
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251251"
---
# <a name="4206---unlockinstanceexception"></a>4206 - UnlockInstanceException

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4206|  
|关键字|WFInstanceStore|  
|Level|错误|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示在尝试解锁某一实例时遇到了异常。  
  
## <a name="message"></a>消息  

 尝试解除实例锁定时遇到异常 %1。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|ExceptionMessage|xs:string|来自 SQL 异常的消息。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
