---
title: Microsoft.Transactions.TransactionBridge.PrepareMessageRetry
ms.date: 03/30/2017
ms.assetid: ada4baa5-b60d-46b8-ad46-4d69f8d8a9fa
ms.openlocfilehash: d5ebe3e1ccce7a85073e2de19d915d116f709b2d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286963"
---
# <a name="microsofttransactionstransactionbridgepreparemessageretry"></a>Microsoft.Transactions.TransactionBridge.PrepareMessageRetry

准备消息重试发送到的参与者无响应。  
  
## <a name="description"></a>描述  

 如果本地事务管理器因为在指定的时间内没有收到响应，而需要向从属参与者重新发行一条 Prepare 消息，就会进行此跟踪。  
  
## <a name="troubleshooting"></a>疑难解答  

 调查导致响应未及时传送的潜在网络或产品问题。  如果看到很多这样的消息，可能表明基础结构有问题或响应时间异常长。 这两种问题都会明显降低系统中事务的吞吐量。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
