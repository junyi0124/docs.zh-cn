---
title: 1016 - CompleteCompletionWorkItem
ms.date: 03/30/2017
ms.assetid: 246929fb-6f14-440a-814b-cd8349350644
ms.openlocfilehash: a192ffe19777ca3e2e9784f6506a0c2929ced000
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275526"
---
# <a name="1016---completecompletionworkitem"></a>1016 - CompleteCompletionWorkItem

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1016|  
|关键字|WFRuntime|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 CompletionWorkItem 已完成。  
  
## <a name="message"></a>消息  

 父 Activity“%1”、DisplayName“'%2”、InstanceId“%3”已完成了 CompletionWorkItem。 已完成的活动“%4”、DisplayName：“%5”、InstanceId：“%6”。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|ParentActivity|xs:string|父活动的类型名称。|  
|ParentDisplayName|xs:string|父活动的显示名称。|  
|ParentInstanceId|xs:string|父活动的实例 ID。|  
|CompletedActivity|xs:string|已完成活动的类型名称。|  
|CompletedActivityDisplayName|xs:string|已完成活动的显示名称。|  
|CompletedActivityInstanceId|xs:string|已完成活动的实例 ID。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
