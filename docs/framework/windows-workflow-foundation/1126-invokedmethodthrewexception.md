---
title: 1126 - InvokedMethodThrewException
ms.date: 03/30/2017
ms.assetid: 0d3cff1a-97e6-4b6c-be18-108c6881bfc0
ms.openlocfilehash: 7caaebe42f49a62fec61ba17a4d3fe3a538e2ab4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262835"
---
# <a name="1126---invokedmethodthrewexception"></a>1126 - InvokedMethodThrewException

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|1126|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 InvokeMethod 活动调用的方法引发了异常。  
  
## <a name="message"></a>消息  

 在活动“%1”调用的方法中，引发了异常。 %2  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|InvokeMethod|xs:string|InvokeMethod 活动的显示名称。|  
|异常|xs:string|异常的异常详细信息|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
