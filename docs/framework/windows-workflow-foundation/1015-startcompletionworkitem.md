---
title: 1015 - StartCompletionWorkItem
ms.date: 03/30/2017
ms.assetid: 96fd1d4e-c5d0-46ad-8a71-4b4b49ac7262
ms.openlocfilehash: c0d8572f192a8faa22327fd671cd9ea49c5054ca
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275539"
---
# <a name="1015---startcompletionworkitem"></a>1015 - StartCompletionWorkItem

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1015|  
|关键字|WFRuntime|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 CompletionWorkItem 正在开始执行。  
  
## <a name="message"></a>消息  

 开始为父 Activity“%1”、DisplayName“%2”、InstanceId“%3”执行 CompletionWorkItem。 已完成的活动“%4”、DisplayName：“%5”、InstanceId：“%6”。  
  
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
