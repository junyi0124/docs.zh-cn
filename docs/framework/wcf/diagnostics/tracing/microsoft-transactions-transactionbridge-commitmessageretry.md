---
title: Microsoft.Transactions.TransactionBridge.CommitMessageRetry
ms.date: 03/30/2017
ms.assetid: 4abe01f0-6398-4fba-b2f3-c054b7f7e971
ms.openlocfilehash: 28b83b293570adf3b1cfdc15c77afd0f0cf768eb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259020"
---
# <a name="microsofttransactionstransactionbridgecommitmessageretry"></a>Microsoft.Transactions.TransactionBridge.CommitMessageRetry

已向无反应参与者发送提交消息重试。  
  
## <a name="description"></a>描述  

 如果本地事务管理器在给定时间内未收到响应，因而需要向从属参与者重新发送“提交”消息，则进行跟踪。  
  
## <a name="troubleshooting"></a>疑难解答  

 调查导致响应未及时传送的潜在网络或产品问题。  如果看到很多这样的消息，可能表明基础结构有问题或响应时间异常长。 这两种问题都会明显降低系统中事务的吞吐量。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
