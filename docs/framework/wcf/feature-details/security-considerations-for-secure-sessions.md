---
title: 安全会话的安全注意事项
ms.date: 03/30/2017
ms.assetid: 0d5be591-9a7b-4a6f-a906-95d3abafe8db
ms.openlocfilehash: abfbb565e06059e05eda3900ba9b8769e3657af8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276030"
---
# <a name="security-considerations-for-secure-sessions"></a>安全会话的安全注意事项

您应考虑实现安全会话时影响安全的下列事项。 有关安全注意事项的详细信息，请参阅安全性 [注意事项](security-considerations-in-wcf.md) 和 [最佳实践](best-practices-for-security-in-wcf.md)。  
  
## <a name="secure-sessions-and-metadata"></a>安全会话和元数据  

 建立安全会话并将 <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.RequireCancellation%2A> 属性设置为时 `false` ，WINDOWS COMMUNICATION FOUNDATION (WCF) 会将 `mssp:MustNotSendCancel` 断言作为服务终结点的 WSDL) 文档 (的元数据的一部分发送。 `mssp:MustNotSendCancel` 断言通知客户端此服务不响应取消安全会话的请求。 当 <xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.RequireCancellation%2A> 属性设置为时 `true` ，WCF 不会 `mssp:MustNotSendCancel` 在 WSDL 文档中发出断言。 当客户端不再需要安全会话时，客户端应向服务发送一个取消请求。 当使用 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)来生成客户端时，客户端代码会适当地响应是否存在 `mssp:MustNotSendCancel` 断言。  
  
## <a name="secure-conversations-and-custom-tokens"></a>安全对话和自定义令牌  

 将自定义令牌和派生密钥混合使用会产生一些问题，这是它在 WS-SecureConversation 规范中的定义方式所导致的。 该规范指出 `wsse:SecurityTokenReference` 是引用派生令牌的可选元素： " `/wsc:DerivedKeyToken/wsse:SecurityTokenReference` 此可选元素用于指定安全上下文令牌、安全令牌或用于派生的共享密钥/机密。 如果未指定，则假定收件人可以从消息上下文确定共享密钥。 如果无法确定上下文，则应该引发诸如之类的错误 `wsc:UnknownDerivationSource` 。 "  
  
 这意味着，如果希望派生自定义令牌，应该将其子句类型包装在 `SecurityTokenReference` 元素中。 有一个选项可关闭派生，但是默认为派生密钥。 如果无法封装密钥，则对派生密钥令牌进行序列化将成功，但是尝试对其进行反序列化将引发异常。  
  
## <a name="see-also"></a>另请参阅

- [如何：在 WSFederationHttpBinding 上禁用安全会话](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)
- [安全注意事项](security-considerations-in-wcf.md)
- [安全性的最佳做法](best-practices-for-security-in-wcf.md)
