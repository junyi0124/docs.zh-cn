---
title: System.ServiceModel.Channels.FailedAcceptFromPool
ms.date: 03/30/2017
ms.assetid: d535b1b5-ee58-45e8-b400-7d9570f4eb9a
ms.openlocfilehash: 8615cca7d4ed8ea1f40baa9502e7136d4349eb9c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256308"
---
# <a name="systemservicemodelchannelsfailedacceptfrompool"></a>System.ServiceModel.Channels.FailedAcceptFromPool

尝试重新使用已入池连接失败。 将在指定的超时期限内再次进行尝试。  
  
## <a name="description"></a>描述  

 此信息跟踪指示尝试重新使用已入池连接时发生错误。 之所以发生此情况，原因在于已入池连接不兼容、未就绪或仍处于活动状态。 在给定的超时范围内将进行重新使用其他已入池连接的更多尝试，如果未找到任何可使用的连接，则会创建新的连接。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
