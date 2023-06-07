---
title: 常用安全方案
ms.date: 03/30/2017
helpviewer_keywords:
- security [WCF], scenarios
ms.assetid: 201923b5-5162-4a8a-8d4c-e7bd242748d5
ms.openlocfilehash: 21c8279890d1d1cf746e98f875efb6b1ff869c73
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295075"
---
# <a name="common-security-scenarios"></a>常用安全方案

本节中的主题对众多可能的客户端和服务安全配置进行分类。 配置会随多种因素而变化。 例如，服务或客户端是否位于 Intranet 上，或者安全性是由 Windows 提供还是由传输（如 HTTPS）提供。  
  
## <a name="in-this-section"></a>本节内容  

 [不安全的 Internet 客户端和服务](internet-unsecured-client-and-service.md)  
 一个公共的、不安全的客户端和服务的示例。  
  
 [不安全的 Intranet 客户端和服务](intranet-unsecured-client-and-service.md)  
 基本 Windows Communication Foundation (WCF) 服务，旨在为 WCF 应用程序提供有关安全的专用网络的信息。  
  
 [通过基本身份验证确保的传输安全](transport-security-with-basic-authentication.md)  
 应用程序允许客户端使用自定义身份验证进行登录。  
  
 [通过 Windows 身份验证确保的传输安全](transport-security-with-windows-authentication.md)  
 显示由 Windows 安全保护的客户端和服务。  
  
 [匿名客户端的传输安全性](transport-security-with-an-anonymous-client.md)  
 此方案使用传输安全（如 HTTPS）确保保密性和完整性。  
  
 [利用证书身份验证的传输安全](transport-security-with-certificate-authentication.md)  
 显示由证书保护的客户端和服务。  
  
 [匿名客户端的消息安全](message-security-with-an-anonymous-client.md)  
 显示由 WCF 消息安全保护的客户端和服务。  
  
 [用户名客户端的消息安全](message-security-with-a-user-name-client.md)  
 客户端是一个 Windows 窗体应用程序，允许客户端使用域用户名和密码登录。  
  
 [使用证书客户端的消息安全](message-security-with-a-certificate-client.md)  
 服务器有多个证书，每个客户端各有一个证书。 通过传输层安全 (TLS) 协商建立安全上下文。  
  
 [Windows 客户端的消息安全](message-security-with-a-windows-client.md)  
 证书客户端的变体。 服务器有多个证书，每个客户端各有一个证书。 通过 TLS 协商建立安全上下文。  
  
 [没有凭据协商的 Windows 客户端的消息安全](message-security-with-a-windows-client-without-credential-negotiation.md)  
 显示由 Kerberos 域保护的客户端和服务。  
  
 [使用相互证书的消息安全](message-security-with-mutual-certificates.md)  
 服务器有多个证书，每个客户端各有一个证书。 服务器证书随应用程序一起分发，而可在带外使用。  
  
 [使用已颁发令牌的消息安全](message-security-with-issued-tokens.md)  
 允许在独立域间建立信任的联合安全。  
  
 [受信任的子系统](trusted-subsystem.md)  
 客户端访问分布在网络上的一个或多个 Web 服务。 Web 服务访问必须加以保护的其他资源（如数据库或其他 Web 服务）。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
## <a name="related-sections"></a>相关章节  

 [授权](authorization-in-wcf.md)  
  
 [安全性概述](security-overview.md)  
  
 [安全性](security.md)  
  
 [绑定与安全](bindings-and-security.md)  
  
 [保护服务和客户端的安全](securing-services-and-clients.md)  
  
 [身份验证](authentication-in-wcf.md)  
  
 [授权](authorization-in-wcf.md)  
  
 [联合令牌与颁发的令牌](federation-and-issued-tokens.md)  
  
 [审核](auditing-security-events.md)  
  
## <a name="see-also"></a>另请参阅

- [安全指导和最佳做法](security-guidance-and-best-practices.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
