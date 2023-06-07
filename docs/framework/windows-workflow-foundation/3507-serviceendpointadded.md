---
title: 3507 - ServiceEndpointAdded
ms.date: 03/30/2017
ms.assetid: c068fc0e-07ee-4551-9824-ea7216e1fe37
ms.openlocfilehash: 903071e7b1f89dc3489b8d3ac05427d699a33a7e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240753"
---
# <a name="3507---serviceendpointadded"></a>3507 - ServiceEndpointAdded

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|3507|  
|关键字|WFServices|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 指示添加了服务终结点。  
  
## <a name="message"></a>消息  

 已为地址“%1”、绑定“%2”和协定“%3”添加了服务终结点。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|地址|xs:string|终结点的地址。|  
|绑定|xs:string|终结点的绑定。|  
|协定|xs:string|终结点的协定。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
