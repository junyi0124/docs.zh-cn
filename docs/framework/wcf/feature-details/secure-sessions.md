---
title: 安全会话
ms.date: 03/30/2017
ms.assetid: 7b50602f-d7b5-42e9-8e92-1f0413df0d8b
ms.openlocfilehash: fd8406af0c37981b2ddc7ab8ddb0c82c63cbc0b1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288537"
---
# <a name="secure-sessions"></a>安全会话

WCF) Windows Communication Foundation (的一项功能是可靠会话，可保证按消息发送顺序接收消息。 本节中的主题讨论在创建可靠会话时要考虑的安全性问题。 有关可靠会话的详细信息，请参阅 [使用会话](../using-sessions.md)。  
  
> [!NOTE]
> 当需要在 Windows XP 上进行模拟时，请不要在安全会话中使用有状态安全令牌上下文 (SCT)。 如果在模拟时使用有状态 SCT，则会引发 <xref:System.InvalidOperationException>。 有关详细信息，请参阅 [不支持的方案](unsupported-scenarios.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
|||  
|-|-|  
|[安全对话和安全会话](secure-conversations-and-secure-sessions.md)|安全对话和安全会话是同义词。 本主题解释安全对话的工作方式以及使用该模式的时机和原因。|  
|[如何：创建安全会话](how-to-create-a-secure-session.md)|演练创建安全会话的基本操作。|  
|[如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)|演练创建能够保持状态以及与客户端之间会话的网络场的步骤。|  
|[安全会话的安全注意事项](security-considerations-for-secure-sessions.md)|描述安全会话的特殊注意事项。|  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
## <a name="related-sections"></a>相关章节  

 [会话、实例化和并发](sessions-instancing-and-concurrency.md)  
  
 [设计和实现服务](../designing-and-implementing-services.md)  
  
## <a name="see-also"></a>另请参阅

- [如何：启用消息重放检测](how-to-enable-message-replay-detection.md)
- [重播攻击](replay-attacks.md)
- [如何：创建要求会话的服务](how-to-create-a-service-that-requires-sessions.md)
