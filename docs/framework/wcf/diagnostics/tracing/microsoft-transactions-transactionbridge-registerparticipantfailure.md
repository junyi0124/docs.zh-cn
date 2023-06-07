---
title: Microsoft.Transactions.TransactionBridge.RegisterParticipantFailure
ms.date: 03/30/2017
ms.assetid: 3a4ead79-8550-4037-84e3-fd70ff56e001
ms.openlocfilehash: 2f0198d3c288b4c3833cdac8e5f943ba822c22e9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96252018"
---
# <a name="microsofttransactionstransactionbridgeregisterparticipantfailure"></a>Microsoft.Transactions.TransactionBridge.RegisterParticipantFailure

WS-AT 协议服务未能注册某个控制协议的参与者。  
  
## <a name="description"></a>描述  

 跟踪 MSDTC 是否检测到无效的注册请求。 这可能是由于多个完成注册请求和内部错误造成的。  
  
## <a name="troubleshooting"></a>疑难解答  

 不要尝试多次“为完成而注册”。  此外，检查跟踪消息中的状态字符串以确定是否存在任何可操作的项。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
