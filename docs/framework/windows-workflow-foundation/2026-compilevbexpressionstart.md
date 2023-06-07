---
title: 2026 - CompileVbExpressionStart
ms.date: 03/30/2017
ms.assetid: daad57eb-8198-49b5-9920-aa0e7428ccf1
ms.openlocfilehash: b264066043716e533f6478deb393b587a69e087c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294542"
---
# <a name="2026---compilevbexpressionstart"></a>2026 - CompileVbExpressionStart

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|2026|  
|关键字|WFRuntime|  
|级别|“详细”|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 Visual Basic 表达式编译的开头。  
  
## <a name="message"></a>消息  

 正在编译 Visual Basic 表达式 "%1"  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|说明|  
|--------------------|--------------------|-----------------|  
|表达式|xs:string|要编译的 VisualBasic 表达式。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
