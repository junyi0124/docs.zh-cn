---
title: 39457 - TrackingRecordRaised
ms.date: 03/30/2017
ms.assetid: 5a2731d1-c731-4b79-bb69-016cb69ef481
ms.openlocfilehash: 5bf343f29528bdb3941e253b2fd5b39799d94c2a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275900"
---
# <a name="39457---trackingrecordraised"></a>39457 - TrackingRecordRaised

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|39457|  
|关键字|WFRuntime|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/调试|  
  
## <a name="description"></a>描述  

 指示 TrackingRecord 已提升为 TrackingParticipant。  
  
## <a name="message"></a>消息  

 跟踪记录 %1 提升为 %2。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|RecordNumber|xs:string|跟踪记录编号。|  
|ParticipantId|xs:string|跟踪参与者。|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
