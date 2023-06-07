---
title: System.ServiceModel.Channels.MsmqMoveOrDeleteAttemptFailed
ms.date: 03/30/2017
ms.assetid: d75d39da-7502-4a6a-91b9-eaa05b8e24d5
ms.openlocfilehash: aeeba38eaf674453f4c87cf62f5088c55b5fde2d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290811"
---
# <a name="systemservicemodelchannelsmsmqmoveordeleteattemptfailed"></a>System.ServiceModel.Channels.MsmqMoveOrDeleteAttemptFailed

无法移动或删除消息。  
  
## <a name="description"></a>描述  

 跟踪指示当尝试移动、删除或拒绝 MSMQ 消息时出错。  
  
 当与 NetMsmqBinding 或 MsmqIntegrationBinding) 一起使用时，MSMQ 消息 Windows Communication Foundation)  ( (WCF。此跟踪与 `ReceiveErrorHandling` NetMsmqBinding 或 MsmqIntegrationBinding 上的属性的选定值相关。  
  
 此跟踪不会指示出全部的系统失败。 但是，它会指示某条消息的选定病毒消息处理失败。 若要详细了解消息何时变为病毒以及如何配置服务以相应地处理这些消息，请参阅 [病毒消息处理](../../feature-details/poison-message-handling.md)。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
