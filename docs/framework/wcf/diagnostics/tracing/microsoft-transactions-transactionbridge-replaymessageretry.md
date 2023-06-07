---
title: Microsoft.Transactions.TransactionBridge.ReplayMessageRetry
ms.date: 03/30/2017
ms.assetid: e5b820ae-504d-405a-926a-9effa41d2369
ms.openlocfilehash: 23a0113488b1b1ef0bc161d4134b9325ef81406c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236710"
---
# <a name="microsofttransactionstransactionbridgereplaymessageretry"></a>Microsoft.Transactions.TransactionBridge.ReplayMessageRetry

已向无响应的协调程序发送重播消息重试。  
  
## <a name="description"></a>描述  

 如果本地事务管理器因未在给定的时间内收到响应而需要向上级协调程序重新发送重播消息，则进行跟踪。  
  
## <a name="troubleshooting"></a>疑难解答  

 调查导致响应未及时传送的潜在网络或产品问题。  如果看到很多这样的消息，可能表明基础结构有问题或响应时间异常长。 这两种问题都会明显降低系统中事务的吞吐量。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
