---
title: 使用安全套接字层
description: 了解 System.Net 和扩展类如何使用安全套接字层对 .NET Framework 中多个网络协议的连接进行加密。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Networking
- SSL
- Secure Sockets Layer
- requesting data from Internet, Secure Sockets Layer
- sending data, Secure Sockets Layer
- Network Resources
- data requests, Secure Sockets Layer
- receiving data, Secure Sockets Layer
- Internet, Secure Sockets Layer
ms.assetid: 6e4289e6-d1b7-4e82-ab0d-e83e3b6063ed
ms.openlocfilehash: 1309e9dc594869cec7bce81ef666d9f5e06f13b9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265175"
---
# <a name="using-secure-sockets-layer"></a>使用安全套接字层

<xref:System.Net> 类使用安全套接字层 (SSL) 为若干网络协议加密连接。  
  
 对于 HTTP 连接，<xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 类使用 SSL 与支持 SSL 的 Web 主机进行通信。 根据为其给定的 URI，<xref:System.Net.WebRequest> 类决定是否使用 SSL。 如果 URI 以“https:”开头，则使用 SSL；如果 URI 以“http:”开头，则使用未加密的连接。  
  
 若要将 SSL 用于文件传输协议 (FTP)，请在调用 <xref:System.Net.FtpWebRequest.GetResponse> 前将 <xref:System.Net.FtpWebRequest.EnableSsl> 属性设置为 true。 同样，要将 SSL 用于简单邮件传输协议 (SMTP)，请在发送电子邮件前将 <xref:System.Net.Mail.SmtpClient.EnableSsl> 属性设置为 true。  
  
 <xref:System.Net.Security.SslStream> 类为 SSL 提供基于流的抽象，并且提供多种配置 SSL 握手的方法。  
  
## <a name="example"></a>示例  
  
### <a name="code"></a>代码  
  
```vb  
Dim MyURI As String = "https://www.contoso.com/"  
Dim Wreq As WebRequest = WebRequest.Create(MyURI)  
  
Dim serverUri As String = "ftp://ftp.contoso.com/file.txt"  
Dim request As FtpWebRequest = CType(WebRequest.Create(serverUri), FtpWebRequest)  
request.Method = WebRequestMethods.Ftp.DeleteFile  
request.EnableSsl = True  
Dim response As FtpWebResponse = CType(request.GetResponse(), FtpWebResponse)  
```  
  
```csharp  
String MyURI = "https://www.contoso.com/";  
WebRequest WReq = WebRequest.Create(MyURI);  
  
String serverUri = "ftp://ftp.contoso.com/file.txt"  
FtpWebRequest request = (FtpWebRequest)WebRequest.Create(serverUri);  
request.EnableSsl = true;  
request.Method = WebRequestMethods.Ftp.DeleteFile;  
FtpWebResponse response = (FtpWebResponse)request.GetResponse();  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
- 引用 System.Net 命名空间。  
  
## <a name="see-also"></a>请参阅

- [网络编程中的安全性](security-in-network-programming.md)
- [.NET Framework 中的网络编程](index.md)
- [证书选择和验证](certificate-selection-and-validation.md)
