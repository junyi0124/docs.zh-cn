---
title: 证书选择和验证
description: 了解 System.Net 类提供的用于选择和验证 SSL/TLS 连接的证书的几种方法。
ms.date: 03/30/2017
ms.assetid: c933aca2-4cd0-4ff1-9df9-267143f25a6f
ms.openlocfilehash: a85b6947c20f7dbced1fb6ad6e79f3134ce1f57b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287509"
---
# <a name="certificate-selection-and-validation"></a>证书选择和验证

<xref:System.Net> 类支持多种针对安全套接字层 (SSL) 连接选择和验证 <xref:System.Security.Cryptography.X509Certificates> 的方法。 客户端可以选择一个或多个证书对服务器自身的客户端进行身份验证。 服务器可以要求客户端证书具有一个或多个用于身份验证的特定属性。  
  
## <a name="definition"></a>定义  

 证书是 ASCII 字节流，包含公钥、属性（如版本号、序列号和到期日期）以及来自证书颁发机构的数字签名。 证书用于建立加密连接，或对服务器的客户端进行身份验证。  
  
## <a name="client-certificate-selection-and-validation"></a>客户端证书选择和验证  

 客户端可以选择一个或多个用于特定 SSL 连接的证书。 客户端证书可以与 Web 服务器或 SMTP 邮件服务器的 SSL 连接相关联。 客户端将证书添加到 <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 或 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 类对象集合。 以电子邮件为例，证书集合是一个与 <xref:System.Net.Mail.SmtpClient> 类的 <xref:System.Net.Mail.SmtpClient.ClientCertificates%2A> 属性相关联的 <xref:System.Security.Cryptography.X509Certificates.X509CertificateCollection> 实例。 <xref:System.Net.HttpWebRequest> 类具有类似的 <xref:System.Net.HttpWebRequest.ClientCertificates%2A> 属性。  
  
 <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 和 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 类的主要区别是私钥需要保存在 <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 类的证书存储中。  
  
 即使证书添加到集合并与特定 SSL 连接相关联，也不会向服务器发送任何证书，除非服务器作出请求。 如果某个连接上设置了多个客户端证书，则将根据算法使用连接性能最好的一个，该算法会考虑由服务器提供的证书颁发者列表与客户端证书颁发者名称之间的匹配。  
  
 <xref:System.Net.Security.SslStream> 类甚至对 SSL 握手提供了更多控制。 客户端可以指定委托选取要使用的客户端证书。  
  
 远程服务器可以验证客户端证书是否有效、是否为最新，以及是否由适当的证书颁发机构签名。 委托可以添加到 <xref:System.Net.ServicePointManager.ServerCertificateValidationCallback%2A> 以强制执行证书验证。  
  
## <a name="client-certificate-selection"></a>客户端证书选择  

 NET Framework 按以下方式选择将提供给服务器的客户端证书：  
  
1. 如果之前已向服务器提供客户端证书，首次提供时，证书将被缓存，并且重复用于后续客户端证书请求。  
  
2. 如果存在委托，请始终使用委托的结果作为客户端证书的选择。 若可能，请尝试使用已缓存的证书，但如果委托已返回 NULL，且证书集合不为空，请勿使用已缓存的匿名凭据。  
  
3. 如果这是对客户端证书的第一次质询，Framework 将枚举与连接关联的 <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 或 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 类对象中的证书，查找由服务器提供的证书颁布者列表与客户端证书颁布者名称之间的匹配。 匹配的第一个证书将被发送到服务器。 如果没有证书匹配或证书集合为空，则向服务器发送匿名凭据。  
  
## <a name="tools-for-certificate-configuration"></a>证书配置工具  

 许多工具都可用于配置客户端和服务器证书。  
  
 Winhttpcertcfg.exe 工具可用于配置客户端证书。 Winhttpcertcfg.exe 工具作为 Windows Server 2003 Resource Kit 的工具之一提供。 该工具在 [www.microsoft.com](https://www.microsoft.com) 上也是作为 Windows Server 2003 Resource Kit 工具的部件可供下载。  
  
HttpCfg.exe 工具可用于配置 <xref:System.Net.HttpListener> 类的服务器证书。 HttpCfg.exe 工具作为 Windows Server 2003 和 Windows XP Service Pack 2 的支持工具之一提供。 默认情况下，Windows Server 2003 或 Windows XP 上都未安装 HttpCfg.exe 和其他支持工具。 在 Windows Server 2003 上， 支持工具单独安装在 Windows Server 2003 CD-ROM 上的以下文件夹和文件中：  
  
 \Support\Tools\Suptools.msi  
  
 若要使用 Windows XP Service Pack 2，可访问 [www.microsoft.com](https://www.microsoft.com) 下载 Windows XP 支持工具。  
  
 HttpCfg.exe 工具版本的源代码也可用作 Windows Server SDK 的示例。 HttpCfg.exe 示例的源代码使用网络示例作为 Windows SDK 的部件在以下文件夹中默认安装：  
  
 C:\Program Files\Microsoft SDKs\Windows\v1.0\Samples\NetDS\http\serviceconfig  
  
 除了这些工具， <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 和 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 类还提供从文件系统加载证书的方法。  
  
## <a name="see-also"></a>请参阅

- [网络编程中的安全性](security-in-network-programming.md)
- [.NET Framework 中的网络编程](index.md)
