---
title: 安全对话和安全会话
ms.date: 03/30/2017
ms.assetid: 48cb104a-532d-40ae-aa57-769dae103fda
ms.openlocfilehash: 6cbf877c80b7d10705868120c4ec4a7b40895114
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288497"
---
# <a name="secure-conversations-and-secure-sessions"></a>安全对话和安全会话

WCF) Windows Communication Foundation (的一项功能是能够在两个终结点之间建立安全会话，这些终结点彼此进行身份验证，并同意加密和数字签名过程。 例如，服务终结点可能要求客户端终结点发送一个基于 X.509 证书的安全令牌，以便进行身份验证。 在对客户端进行身份验证之后，服务终结点将向客户端返回一个安全上下文令牌 (SCT)，该令牌随后将用于保护该会话中的所有后续消息。 通过建立这一安全会话，可使两个终结点之间交换的一组消息更有效率，因为 SCT 具有对称密钥。 在生成数字签名或加密一组数据时，与对称密钥相比，作为 X.509 证书基础的非对称密钥明显需要更多的计算能力。  
  
 [Ws-securitypolicy](https://docs.oasis-open.org/ws-sx/ws-securitypolicy/200702/ws-securitypolicy-1.2-spec-os.html)) 标准的6.2.7 部分中定义的启动策略 (包含用于保护通道的消息安全断言，并在 RST/SCT 和 RSTR/sct 交换之前对客户端进行身份验证。 某些 WCF 标准绑定具有一个 `Security.Message.EstablishSecurityContext` 属性，该属性控制是否使用安全对话。 使用自定义绑定时，可以通过 [\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 在配置文件中或通过 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateSecureConversationBindingElement%2A> 在代码中调用来嵌套安全绑定元素，来指示启动。  
  
 有关会话的详细信息，请参阅 [使用会话](../using-sessions.md)。  
  
## <a name="see-also"></a>另请参阅

- [会话、实例化和并发](sessions-instancing-and-concurrency.md)
- [如何：创建要求会话的服务](how-to-create-a-service-that-requires-sessions.md)
