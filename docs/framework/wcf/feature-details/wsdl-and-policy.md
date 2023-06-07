---
title: WSDL 和策略
ms.date: 03/30/2017
ms.assetid: cea87440-3519-4640-8494-b8a2b0e88c84
ms.openlocfilehash: 123a878e90ee9099b009985a5e79155e8b1cd097
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238335"
---
# <a name="wsdl-and-policy"></a>WSDL 和策略

本主题介绍 Windows Communication Foundation (WCF) WSDL 1.1、WS-Policy 和 WS-PolicyAttachment 实现详细信息，以及 WCF 引入的其他 WS-Policy 断言和 WSDL 1.1 扩展。  
  
 WCF 通过本文档中所述的约束和说明，实现提交给 W3C 的 WS-Policy 和 WS-PolicyAttachment 规范。  
  
 本文档使用下表中所示的前缀和命名空间。  
  
|前缀|命名空间|  
|------------|---------------|  
|wsp (WS-Policy 1.2)|`http://schemas.xmlsoap.org/ws/2004/09/policy`|  
|wsp (WS-Policy 1.5)|`http://www.w3.org/ns/ws-policy`|  
|http|`http://schemas.microsoft.com/ws/06/2004/policy/http`|  
|msmq|`http://schemas.microsoft.com/ws/06/2004/mspolicy/msmq`|  
|msf|`http://schemas.microsoft.com/ws/2006/05/framing/policy`|  
|mssp|`http://schemas.microsoft.com/ws/2005/07/securitypolicy`|  
|msc|`http://schemas.microsoft.com/ws/2005/12/wsdl/contract`|  
|cdp|`http://schemas.microsoft.com/net/2006/06/duplex`|  
  
## <a name="wcf-wsdl11-extensions"></a>WCF WSDL1.1 扩展  

 WCF 使用以下 WSDL 1.1 扩展来描述协定会话要求。  
  
 wsdl:portType/wsdl:operation/@msc:isInitiating  
 xs： boolean，指示此操作启动 WCF 会话;默认值为 `false` 。  
  
 wsdl:portType/wsdl:operation/@msc:isTerminating  
 xs： boolean，指示此操作终止 WCF 会话;默认值为 `false` 。  
  
 wsdl:portType/wsdl:operation/@msc:usingSession  
 xs:boolean，指示此协定要求建立会话。  
  
### <a name="soap-1x-http-binding-transport-uris"></a>SOAP 1.x HTTP 绑定传输 URI  

 WCF 使用以下 Uri 来指示要用于 WSDL 1.1、SOAP 1.1 和 SOAP 1.2 绑定扩展元素的传输。  
  
|Transport|URI|  
|---------------|---------|  
|HTTP|`http://schemas.xmlsoap.org/soap/http`|  
|TCP|`http://schemas.microsoft.com/soap/tcp`|  
|MSMQ|`http://schemas.microsoft.com/soap/msmq`|  
|命名管道|`http://schemas.microsoft.com/soap/named-pipe`|  
  
## <a name="policy-assertions-implemented-by-wcf"></a>由 WCF 实现的策略断言  

 除了在 Web Services 规范中引入的策略断言 (WS-ATOMICTRANSACTION ) 并在本文档的其他部分中提到，WCF 还实现了以下策略断言。  
  
|策略断言|策略主题|描述|  
|----------------------|--------------------|-----------------|  
|http:HttpBasicAuthentication|终结点|终结点使用 HTTP 基本身份验证。|  
|http:HttpDigestAuthentication|终结点|终结点使用 HTTP 摘要式身份验证。|  
|http:HttpNegotiateAuthentication|终结点|终结点使用 HTTP 协商身份验证。|  
|http:HttpNtlmAuthentication|终结点|终结点使用 HTTP NTLM 身份验证。|  
|msf:Streamed|终结点|终结点使用经过流式处理的消息组帧。 此断言与为诸如 TCP 之类的传输以及命名管道提供的消息组帧协议一起使用。|  
|msf:SslTransportSecurity|终结点|终结点将传输层安全 (TLS) 与消息组帧技术一起使用。|  
|msf:WindowsTransportSecurity|终结点|终结点将安全提供程序协商 (SPNEGO) 与消息组帧一起使用。|  
|msmq:MsmqBestEffort|终结点|具有最大努力保证的 MSMQ。|  
|msmq:MsmqSession|终结点|具有会话保证的 MSMQ。|  
|msmq:MsmqVolatile|终结点|可变 MSMQ。|  
|msmq:Authenticated|终结点|将身份验证与 MSMQ 传输一起使用。|  
|msmq:WindowsDomain|终结点|MSMQ 使用 Windows 域身份验证。|  
|cdp:CompositeDuplex|终结点|终结点将两个独立且逆向的传输连接分别用于传入消息和传出消息。|  
|mssp:RsaToken|嵌套|RSA 密钥令牌断言。 通常由作为认可签名中密钥信息的一部分直接序列化的 RSA 密钥来满足此要求。|  
|mssp:SslContextToken|嵌套|要求使用通过利用 WS-Trust 的 TLS 握手获取的 SecurityContextToken。 嵌套断言包括：sp:RequireDerivedKeys、mssp:MustNotSendCancel、mssp:RequireClientCertificate。|  
|mssp:MustNotSendCancel|嵌套|指定一个需求，即不要将使用 Cancel 绑定 [WS-Trust、WS-SC] 的请求安全令牌 (RST) 请求消息 [WS-Trust] 发送给给定 SecurityContextToken 的颁发机构。 如果此断言存在，则不得将此类请求消息发送给颁发机构。 如果此断言不存在，则可以将此类请求消息发送给颁发机构。|  
|mssp:RequireClientCertificate|嵌套|这一可选元素指定需要作为 TLSNEGO 协议的一部分提供的客户端证书。 如果此断言存在，则必须提供客户端证书。 如果此断言不存在，则不得提供客户端证书。 此断言不得在 mssp:SslContextToken 外部使用。|  
  
## <a name="see-also"></a>另请参阅

- [自定义 WSDL 发布](../samples/custom-wsdl-publication.md)
- [如何：导出自定义 WSDL](../extending/how-to-export-custom-wsdl.md)
- [如何：导入自定义 WSDL](../extending/how-to-import-custom-wsdl.md)
