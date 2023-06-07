---
title: Internet 身份验证
description: 了解 System.Net 类支持 .NET Framework 中的应用程序的各种客户端身份验证机制。
ms.date: 03/30/2017
helpviewer_keywords:
- authentication [.NET Framework], classes
- IAuthenticationModule interface
- ICredentialLookup interface
- CredentialCache class, about CredentialCache class
- receiving data, authentication
- AuthenticationManager class, about AuthenticationManager class
- Internet, authentication
- sending data, authentication
- network resources, authentication
- user authentication, classes for authentication
- NetworkCredential class, about NetworkCredential class
- client authentication, classes for authentication
ms.assetid: d342e87c-f672-4660-a513-41a2f2b80c4a
ms.openlocfilehash: 085ca27dd0cfedc90211b21c10cc8bc5cf1ecd21
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241585"
---
# <a name="internet-authentication"></a>Internet 身份验证

<xref:System.Net> 类支持多种客户端身份验证机制，包括标准 Internet 身份验证方法、基本、摘要式、协商、NTLM 和 Kerberos 身份验证，以及可以创建的自定义方法。  
  
 身份验证凭据存储在实现 <xref:System.Net.ICredentials> 接口的 <xref:System.Net.NetworkCredential> 和 <xref:System.Net.CredentialCache> 类中。 在查询其中一个类以获取凭据时，它会返回 NetworkCredential 类的实例。 身份验证过程由 <xref:System.Net.AuthenticationManager> 类管理，而实际的身份验证过程由实现 <xref:System.Net.IAuthenticationModule> 接口的身份验证模块类执行。 必须首先向 AuthenticationManager 注册自定义身份验证模块才能使用此模块；基本、摘要式、协商、NTLM 和 Kerberos 身份验证方法的模块是默认注册的。  
  
 NetworkCredential 存储与由 URI 标识的单个 Internet 资源相关联的凭据集，并返回它们以响应任何对 <xref:System.Net.NetworkCredential.GetCredential%2A> 方法的调用。 NetworkCredential 类通常由访有限数量的 Internet 资源的应用程序使用，或者由在所有情况下使用同一凭据集的应用程序使用。  
  
 CredentialCache 类存储各种 Web 资源的凭据集合。 调用 <xref:System.Net.CredentialCache.GetCredential%2A> 方法时，CredentialCache 会返回适合的凭据集，由 Web 资源的 URI 以及请求的身份验证方案来确定。 通过不同的身份验证方案使用各种 Internet 资源的应用程序可从使用 CredentialCache 中受益，因为此类可以存储所有凭据，并按照请求提供这些凭据。  
  
 Internet 资源请求身份验证时，<xref:System.Net.WebRequest.GetResponse%2A?displayProperty=nameWithType> 方法将 <xref:System.Net.WebRequest> 发送给 AuthenticationManager，同时会发送对凭据的请求。 之后会根据以下过程对请求进行身份验证：  
  
1. AuthenticationManager 按照注册的顺序在每个已注册的身份验证模块上调用 <xref:System.Net.IAuthenticationModule.Authenticate%2A> 方法。 AuthenticationManager 使用第一个不返回 null 的模块来执行身份验证过程 。 此过程的详细信息根据所涉及到的身份验证模块类型而异。  
  
2. 完成身份验证过程后，此身份验证模块向包含访问 Internet 资源所需的信息的 WebRequest 返回 <xref:System.Net.Authorization>。  
  
 某些身份验证方案可以对用户进行身份验证，而无需首先对资源发出请求。 应用程序可以使用资源预先对用户进行身份验证以节省时间，这样可以减少至少到服务器的一个往返。 或者，它也可以在程序启动期间执行身份验证，便于稍后更好地响应用户。 可以使用预身份验证的身份验证方案将 <xref:System.Net.IAuthenticationModule.PreAuthenticate%2A> 属性设置为“true”。  
  
## <a name="see-also"></a>请参阅

- [基本和摘要式身份验证](basic-and-digest-authentication.md)
- [NTLM 和 Kerberos 身份验证](ntlm-and-kerberos-authentication.md)
- [网络编程中的安全性](security-in-network-programming.md)
