---
title: 1035 - RuntimeTransactionSet
ms.date: 03/30/2017
ms.assetid: 03b37de9-778c-4beb-b0e3-de73ece6088e
ms.openlocfilehash: b8bf8431c4e2b82c6aac95820eb45de2a404e976
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294269"
---
# <a name="1035---runtimetransactionset"></a>1035 - RuntimeTransactionSet

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1035|  
|关键字|WFRuntime|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示活动已设置为运行时事务。  
  
## <a name="message"></a>消息  

 运行时事务已由 Activity "%1"、DisplayName "%2"、InstanceId "%3" 设置。  执行操作与 Activity "%4"、DisplayName "%5"、InstanceId "%6" 隔离。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|活动|xs:string|活动的类型名称。|  
|DisplayName|xs:string|活动的显示名称。|  
|InstanceId|xs:string|活动的实例 ID。|  
|IsolatedActivity|xs:string|事务隔离到的活动的类型名称。|  
|IsolatedActivityDisplayName|xs:string|事务隔离到的活动的显示名称。|  
|IsolatedActivityInstanceId|xs:string|事务隔离到的活动的实例 ID。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
