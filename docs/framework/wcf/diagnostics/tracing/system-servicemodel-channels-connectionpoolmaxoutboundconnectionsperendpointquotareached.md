---
title: Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure
ms.date: 03/30/2017
ms.assetid: 1b9f5139-e122-4716-9ef7-2f38e1813993
ms.openlocfilehash: 3e4e1ff7ea8d8768c8d902dc09bd3b78646c2caf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256321"
---
# <a name="microsofttransactionstransactionbridgeenlisttransactionfailure"></a>Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure

WS-AT 协议服务无法使用提供的协调上下文在事务上登记。  
  
## <a name="description"></a>描述  

 对于给定的 2pc 协议，当 MSDTC 无法在事务上登记时跟踪。  当事务不再存在、不再允许登记或者已存在过多的登记项时可能出现这种情况。  
  
## <a name="troubleshooting"></a>疑难解答  

 检查跟踪消息中的状态字符串以确定是否存在任何可操作的项。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
