---
title: 1027 - StartTransactionContextWorkItem
ms.date: 03/30/2017
ms.assetid: 116ae5ec-b9d5-4231-824e-270d00eea7b8
ms.openlocfilehash: cb5671ce7a30a7096104ba0ca6c4f36bed6b93f9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275227"
---
# <a name="1027---starttransactioncontextworkitem"></a>1027 - StartTransactionContextWorkItem

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1027|  
|关键字|WFRuntime|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 TransactionContextWorkItem 正在开始执行。  
  
## <a name="message"></a>消息  

 开始为 Activity“%1”、DisplayName“%2”、InstanceId“%3”执行 TransactionContextWorkItem。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|活动|xs:string|活动的类型名称。|  
|DisplayName|xs:string|活动的显示名称。|  
|InstanceId|xs:string|活动的实例 ID。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
