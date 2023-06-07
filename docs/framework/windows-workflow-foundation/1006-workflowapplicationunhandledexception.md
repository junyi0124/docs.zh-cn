---
title: 1006 - WorkflowApplicationUnhandledException
ms.date: 03/30/2017
ms.assetid: dfe0f316-e03b-47fb-b6a3-622c56bfd753
ms.openlocfilehash: 4bb76a93ec7a07a735def1f1d5dc79decb7792ad
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239828"
---
# <a name="1006---workflowapplicationunhandledexception"></a>1006 - WorkflowApplicationUnhandledException

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1006|  
|关键字|WFRuntime|  
|Level|错误|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示工作流应用程序遇到了未经处理的异常。  
  
## <a name="message"></a>消息  

 WorkflowInstance Id "%1" 遇到未经处理的异常。  此异常源自 Activity "%2"、DisplayName "%3"。  将执行以下操作： %4。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|WorkflowInstanceId|`xs:string`|工作流的实例 ID|  
|异常|`xs:string`|异常的异常详细信息|  
|应用程序域|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
