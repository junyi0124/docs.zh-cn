---
title: Microsoft.Transactions.TransactionBridge.VolatileOutcomeTimeout
ms.date: 03/30/2017
ms.assetid: 2dbe34c5-57c7-4b64-9257-63021911d03c
ms.openlocfilehash: bc2cdc2221ec522b44c36ef3320b77124d61850e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236697"
---
# <a name="microsofttransactionstransactionbridgevolatileoutcometimeout"></a>Microsoft.Transactions.TransactionBridge.VolatileOutcomeTimeout

WS-AT 协议服务在等待可变参与方对结果消息的响应时超时。 如果参与方返回，则事务结果可能不确定。  
  
## <a name="description"></a>描述  

 当可变参与方已决定提交或中止，但未在给定时间内响应提交或回滚请求时跟踪。  
  
## <a name="troubleshooting"></a>疑难解答  

 确保所有可变参与方都能在给定时间内响应。 默认时间为 180 秒。  如果此时间不够，请增加 WS-AT 的 `VolatileOutcomeDelay` 计时器策略。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
