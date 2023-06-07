---
title: 3501 - InferredContractDescription
ms.date: 03/30/2017
ms.assetid: 21a70849-4fc0-41d2-b9a4-db5aa2acdf1a
ms.openlocfilehash: 88a04c0eb6d12876592702ad4dba3a17aa8da122
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245284"
---
# <a name="3501---inferredcontractdescription"></a>3501 - InferredContractDescription

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|3501|  
|关键字|WFServices|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 指示已从 WorkflowService 中推断出 ContractDescription。  
  
## <a name="message"></a>消息  

 已从 WorkflowService 中推断出含有 Name 为“%1”和 Namespace 为“%2”的 ContractDescription。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|“属性”|xs:string|ContractDescription 的名称。|  
|命名空间|xs:string|ContractDescription 的命名空间。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
