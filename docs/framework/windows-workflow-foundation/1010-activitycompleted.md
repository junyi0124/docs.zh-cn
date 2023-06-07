---
title: 1010 - ActivityCompleted
ms.date: 03/30/2017
ms.assetid: d256284e-3fd2-4c33-82f4-abb617575706
ms.openlocfilehash: d0ebf32ec1d5fe5b34ffe650d5547891be0eb665
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239908"
---
# <a name="1010---activitycompleted"></a>1010 - ActivityCompleted

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1010|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示活动已完成执行。  
  
## <a name="message"></a>消息  

 Activity“%1”、DisplayName“%2”、InstanceId“%3”在“%4”状态下完成。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|活动|xs:string|活动的类型名称。|  
|DisplayName|xs:string|活动的显示名称。|  
|InstanceId|xs:string|活动的实例 ID。|  
|状态|xs:string|活动的状态。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
