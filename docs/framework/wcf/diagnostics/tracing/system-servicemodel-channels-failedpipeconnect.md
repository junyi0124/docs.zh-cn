---
title: System.ServiceModel.Channels.FailedPipeConnect
ms.date: 03/30/2017
ms.assetid: 9a827e0f-fb91-46bb-bd54-926d4b74d8a6
ms.openlocfilehash: b3b34b2f4ab2987360030d614c8e65cd878d4d14
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256243"
---
# <a name="systemservicemodelchannelsfailedpipeconnect"></a>System.ServiceModel.Channels.FailedPipeConnect

尝试连接到指定的管道终结点失败。 将在指定的超时期限内再次进行尝试。  
  
## <a name="description"></a>描述  

 此信息跟踪指示未能连接到指定的管道终结点。 如果未找到指定的管道终结点或指定的管道终结点正忙，则可能会发生此情况。 将进行更多的尝试（两次尝试之间的时间较短），直到尝试成功或 OpenTimeout 过期。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
