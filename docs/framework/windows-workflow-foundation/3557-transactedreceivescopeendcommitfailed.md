---
title: 3557 - TransactedReceiveScopeEndCommitFailed
ms.date: 03/30/2017
ms.assetid: 079f0188-8146-49ee-b6ae-a08f4e4d2b9b
ms.openlocfilehash: 4a4979047481687ef0d5c9d5891dec8f2826beed
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263654"
---
# <a name="3557---transactedreceivescopeendcommitfailed"></a>3557 - TransactedReceiveScopeEndCommitFailed

## <a name="properties"></a>属性  
  
|||  
|-|-|  
|ID|3557|  
|关键字|WFServices|  
|Level|信息|  
|通道|Microsoft-Windows-应用程序服务器-应用程序/分析|  
  
## <a name="description"></a>描述  

 指示对 CommittableTransaction 上的 EndCommit 的调用引发 TransactionException。  
  
## <a name="message"></a>消息  

 ID 为“%1”的 CommittableTransaction 上的 EndCommit 调用引发了具有以下消息的 TransactionException:“%2”。  
  
## <a name="details"></a>详细信息  
  
|数据项名称|数据项类型|描述|  
|--------------------|--------------------|-----------------|  
|TransactionId|xs:string|CommittableTransaction 的 ID。|  
|异常|xs:string|异常的异常详细信息|  
|应用程序域|xs:string|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|
