---
title: System.ServiceModel.Channels.PeerFlooderReceiveMessageQuotaExceeded
ms.date: 03/30/2017
ms.assetid: b8371d0a-843e-440b-b86a-6996db131cb0
ms.openlocfilehash: d4fbbeaaa1daeea62b5495d0b30c37d0297ccd75
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249912"
---
# <a name="systemservicemodelchannelspeerflooderreceivemessagequotaexceeded"></a>System.ServiceModel.Channels.PeerFlooderReceiveMessageQuotaExceeded

消息的入站接收速率过高。  
  
## <a name="description"></a>描述  

 此跟踪在尝试处理入站消息时发生。 无法将消息转发给特定的邻居，因为已经超过了为该邻居设置的配额。 当无响应邻居无法清除为该邻居挂起的积压工作 (backlog) 消息时会发生此情况。  
  
## <a name="troubleshooting"></a>疑难解答  

 降低在网格中发送消息的速率。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
