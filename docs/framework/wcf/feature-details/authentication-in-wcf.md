---
title: WCF 中的身份验证
description: 了解 WCF 中几个提供身份验证的机制，例如 Windows 身份验证、x.509 证书、用户名和密码。
ms.date: 03/30/2017
helpviewer_keywords:
- authentication [WCF]
- security [WCF], authentication
ms.assetid: 9254d873-843d-4c6e-bea4-8184ac3e44f4
ms.openlocfilehash: e2334a8c024238f38e1c927a278a4e25e7dabd9d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247533"
---
# <a name="authentication-in-wcf"></a>WCF 中的身份验证

以下主题介绍了 Windows Communication Foundation (WCF) 提供身份验证的多种不同机制，这些机制提供身份验证，例如 Windows 身份验证、x.509 证书以及用户名和密码。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：使用 ASP.NET 成员资格提供程序](how-to-use-the-aspnet-membership-provider.md)  
 ASP.NET 功能包括成员资格和角色提供程序、用于存储用户名/密码对以供进行身份验证的数据库，以及用于身份验证的用户角色。 本主题说明 WCF 服务如何使用同一个数据库对用户进行身份验证和授权。  
  
 [如何：使用自定义用户名和密码验证程序](how-to-use-a-custom-user-name-and-password-validator.md)  
 演示如何集成自定义用户名/密码验证程序。  
  
 [服务标识和身份验证](service-identity-and-authentication.md)  
 作为额外的安全措施，客户端可以通过指定服务的预期 *标识* 来对服务进行身份验证。 如果期望的标识与服务返回的标识不匹配，则身份验证失败。  
  
 [安全协商和超时](security-negotiation-and-timeouts.md)  
 说明如何使用 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.NegotiationTimeout%2A> 类的 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings> 属性。  
  
 [调试 Windows 身份验证错误](debugging-windows-authentication-errors.md)  
 重点说明使用 Windows 身份验证时所遇到的常见问题。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
## <a name="related-sections"></a>相关章节  

 [常用安全方案](common-security-scenarios.md)  
  
## <a name="see-also"></a>另请参阅

- [安全性概述](security-overview.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
