---
title: 4205 - FoundProcessingError
ms.date: 03/30/2017
ms.assetid: f2235a15-dd87-439e-8cb9-8b1b89a3dacf
ms.openlocfilehash: 2931d3723a04d7970197c9ebd79dc65ea43d67a7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251238"
---
# <a name="4205---foundprocessingerror"></a>4205 - FoundProcessingError

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|4205|  
|关键字|WFInstanceStore|  
|Level|错误|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 SQL 提供程序命令已失败。  
  
## <a name="message"></a>消息  

 命令失败: %1  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|ExceptionMessage|xs:string|来自 SQL 异常的消息。|  
|异常|xs:string|异常的异常详细信息|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
