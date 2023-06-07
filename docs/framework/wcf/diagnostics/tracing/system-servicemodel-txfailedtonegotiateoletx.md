---
title: System.ServiceModel.TxFailedToNegotiateOleTx
ms.date: 03/30/2017
ms.assetid: 3f0f0b4b-a1ad-4704-8329-455daf54892d
ms.openlocfilehash: 7b468a57409e9a473a5bbf8e3324435e65e8c60b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295309"
---
# <a name="systemservicemodeltxfailedtonegotiateoletx"></a>System.ServiceModel.TxFailedToNegotiateOleTx

对指定协调上下文的 OleTransactions 协议协商失败。  
  
## <a name="description"></a>描述  

 当带有 OleTx 信息的传入事务无法使用附加的 OleTx 信息，将回退并改用 WS-AT 时进行跟踪。  
  
## <a name="troubleshooting"></a>疑难解答  

 指示计算机之间的 MSDTC RPC 通信所具有的一个潜在问题。 如果日志中出现许多此类跟踪，则可能导致性能急剧下降。  如果不希望使用 OleTx，请在 WS-AT 注册表配置中将 `OleTxUpgradeEnabled` 设置为 0。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
