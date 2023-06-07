---
title: 4203 - RenewLockSystemError
ms.date: 03/30/2017
ms.assetid: 6ec9ec6f-4ae2-45cf-b99b-02cdb9dc9ec9
ms.openlocfilehash: 17617e25c5cf8cecae608438529e9ce1a7d506f7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251264"
---
# <a name="4203---renewlocksystemerror"></a>4203 - RenewLockSystemError

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4203|  
|关键字|WFInstanceStore|  
|Level|错误|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 SQL 提供程序由于锁定到期日已过或者已删除锁定所有者，未能延长锁定到期日。 SqlWorkflowInstanceStore 将被中止。  
  
## <a name="message"></a>消息  

 未能延长锁定到期日，锁定到期日已过，或者已删除锁定所有者。 正在中止 SqlWorkflowInstanceStore。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
