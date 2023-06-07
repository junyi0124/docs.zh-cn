---
title: 重播攻击
ms.date: 03/30/2017
ms.assetid: 7a17e040-93cd-4432-81b9-9f62fec78c8f
ms.openlocfilehash: 4325b3747074f13cf02752f99b25fa02e4117b4c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239076"
---
# <a name="replay-attacks"></a>重播攻击

当攻击者在两个参与方之间复制消息流并将该流重播给一个或多个参与方时，会发生 *重播攻击* 。 除非攻击得到缓解，否则，受到攻击的计算机会将该消息流作为合法消息进行处理，从而导致一系列不良后果，例如重复订购某种产品。  
  
## <a name="bindings-may-be-subject-to-reflection-attacks"></a>绑定可能会遭受反射攻击  

 *反射攻击* 是将消息重播回发送方，就好像它们来自接收方作为回复。 Windows Communication Foundation 中的标准 *重播检测* (WCF) 机制不会自动处理这种情况。  
  
 默认情况下，将减轻反射攻击，因为 WCF 服务模型会添加签名的消息 ID 来请求消息，并在响应消息中要求使用签名 `relates-to` 标头。 因此，请求消息无法作为响应进行重播。 在安全的可靠消息 (RM) 方案中，反射攻击会由于以下原因而得到缓解：  
  
- 创建序列架构和创建序列响应消息架构是不同的。  
  
- 对于单工序列，无法将客户端发送的序列消息重播回客户端，因为客户端无法理解这样的消息。  
  
- 对于双工序列，这两个序列 ID 必须是唯一的。 因此，无法将传出序列消息作为传入序列消息重播回发送方（所有序列标头和正文也都进行了签名）。  
  
 唯一容易遭受反射攻击的绑定是那些不使用 WS-Addressing 的绑定，即那些禁用了 WS-Addressing 的自定义绑定以及那些使用基于对称密钥的安全的绑定。 默认情况下，<xref:System.ServiceModel.BasicHttpBinding> 不使用 WS-Addressing，但是它不会以使其容易遭受反射攻击的方式使用基于对称密钥的安全。  
  
 自定义绑定的缓解是不建立安全上下文或要求使用 WS-Addressing 标头。  
  
## <a name="web-farm-attacker-replays-request-to-multiple-nodes"></a>网络场：攻击者向多个节点重播请求  

 客户端使用在网络场中实现的服务。 攻击者将发送到网络场中某个节点的请求重播到该网络场中的其他节点。 另外，如果重新启动服务，则会刷新重播缓存，从而使攻击者可以重播请求。 （该缓存包含已使用过的、先前已见到的消息签名值并可以防止重播，因此这些签名只能使用一次。 重播缓存不在整个网络场中共享。）  
  
 缓解措施包括：  
  
- 将消息模式安全与有状态安全上下文令牌结合使用（启用或不启用安全对话）。 有关详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)。  
  
- 将服务配置为使用传输级安全。  
  
## <a name="see-also"></a>另请参阅

- [安全注意事项](security-considerations-in-wcf.md)
- [信息泄露](information-disclosure.md)
- [权限提升](elevation-of-privilege.md)
- [拒绝服务](denial-of-service.md)
- [篡改](tampering.md)
- [不支持的方案](unsupported-scenarios.md)
