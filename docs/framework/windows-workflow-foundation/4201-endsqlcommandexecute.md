---
title: 4201 - EndSqlCommandExecute
ms.date: 03/30/2017
ms.assetid: ae0dbc15-f98c-4096-a8d9-fbe4dc36f1cd
ms.openlocfilehash: 0d6326889077e36ad49aa6267ae7285849c6818d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275861"
---
# <a name="4201---endsqlcommandexecute"></a>4201 - EndSqlCommandExecute

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4201|  
|关键字|WFInstanceStore|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 SQL 命令已完成执行。  
  
## <a name="message"></a>消息  

 结束 SQL 命令执行: %1  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|SqlCommand|xs:string|已执行的 SQL 命令。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
