---
title: 联合令牌与颁发的令牌
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, federation
- issued tokens [WCF]
- federation [WCF], issued tokens
ms.assetid: 4c31ee7d-a820-4067-8b84-a83049021bb6
ms.openlocfilehash: 0ab3d6bad717e71901b4d94c99f1c48f99d8675e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255515"
---
# <a name="federation-and-issued-tokens"></a>联合令牌与颁发的令牌

利用 Windows Communication Foundation (WCF) ，可以创建与实现 WS-Federation 和 WS-Trust 规范的服务安全通信的客户端。 这些规范使用 XML、SOAP 和 Web 服务描述语言 (WSDL) 来提供用来跨不同的信任领域进行身份验证和授权的机制。  
  
## <a name="in-this-section"></a>本节内容  

 [联合](federation.md)  
 提供对联合的概述。  
  
 [联合与信任](federation-and-trust.md)  
 列出在创建联合服务或客户端时应注意的设计问题。  
  
 [如何：创建联合客户端](how-to-create-a-federated-client.md)  
 介绍使用 WCF 创建联合客户端的基础知识。  
  
 [如何：在联合身份验证服务上配置凭据](how-to-configure-credentials-on-a-federation-service.md)  
 描述创建联合服务的步骤。  
  
 [如何：创建 WSFederationHttpBinding](how-to-create-a-wsfederationhttpbinding.md)  
 描述如何配置使用 `WSFederationHttpBinding` 的客户端和服务。  
  
 [如何：创建安全令牌服务](how-to-create-a-security-token-service.md)  
 描述创建安全令牌服务的步骤。  
  
 [安全断言标记语言 (SAML) 令牌和声明](saml-tokens-and-claims.md)  
 描述可扩展并能够用来创建丰富的声明类型的安全断言标记语言 (SAML) 令牌。  
  
 [如何：配置本地颁发者](how-to-configure-a-local-issuer.md)  
 描述如何创建安全令牌的本地颁发机构。  
  
 [如何：在 WSFederationHttpBinding 上禁用安全会话](how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)  
 描述如何在 `WSFederationHttpBinding` 上禁用安全会话。 在创建要求每个客户端都有一个会话的网络场时，有必要禁用安全会话。  
  
## <a name="reference"></a>参考  

 <xref:System.IdentityModel.Claims>  
  
 <xref:System.ServiceModel.ServiceAuthorizationManager>  
  
 <xref:System.IdentityModel.Tokens.SamlSecurityToken>  
  
 <xref:System.ServiceModel.Security.IssuedTokenClientCredential>  
  
 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential>  
  
 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters>  
  
 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenProvider>  
  
 <xref:System.ServiceModel.WSFederationHttpBinding>  
  
## <a name="see-also"></a>请参阅

- [授权](authorization-in-wcf.md)
- [自定义令牌](../extending/custom-tokens.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
