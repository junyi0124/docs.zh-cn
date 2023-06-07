---
title: 处理错误
description: 了解由 WebRequest 和 WebResponse 引发的系统和特定于 Web 的异常。 使用 Status 属性来了解和解决问题。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Internet, WebRequest and WebResponse classes exceptions
- Status property
- WebExceptions class, about WebExceptions class
- Timeout enumeration member
- ConnectFailure enumeration member
- TrustFailure enumeration member
- WebRequest class, exceptions
- requesting data from Internet, error handling
- Success enumeration member
- receiving data, errors
- ProtocolError enumeration member
- downloading Internet resources, error handling
- WebResponse class, exceptions
- SendFailure enumeration member
- errors [.NET Framework], WebRequest and WebResponse classes exceptions
- sending data, errors
- response to Internet request, error handling
- NameResolutionFailure enumeration member
- KeepAliveFailure enumeration member
- network resources, WebRequest and WebResponse classes exceptions
- RequestCanceled enumeration member
- ReceiveFailure enumeration member
- ServerProtocolViolation enumeration member
- ConnectionClosed enumeration member
- SecureChannelFailure enumeration member
ms.assetid: 657141cd-5cf5-4fdb-a4b2-4c040eba84b5
ms.openlocfilehash: 786b2bd8bc4d1b394bcfe920053b2f4f55d1cdea
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502569"
---
# <a name="handling-errors"></a>处理错误

<xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 类都会引发系统异常（如 <xref:System.ArgumentException>）和特定于 Web 的异常（为 <xref:System.Net.WebRequest.GetResponse%2A> 方法所引发的 <xref:System.Net.WebException>）。  
  
每个 WebException 包括一个 <xref:System.Net.WebException.Status%2A> 属性，它包含来自 <xref:System.Net.WebExceptionStatus> 枚举的一个值。 可以检查“状态”属性以确定发生的错误并采取适当的步骤解决该错误。  
  
下表描述了“状态”属性可能的值。  
  
|状态|描述|  
|------------|-----------------|  
|ConnectFailure|无法在传输级别联系远程服务。|  
|ConnectionClosed|连接被提前关闭。|  
|KeepAliveFailure|服务器已关闭与“保持的连接”标头设置建立的连接。|  
|NameResolutionFailure|名称服务无法解析主机名。|  
|ProtocolError|从服务器接收的响应是完整的，但指示在协议级别出现错误。|  
|ReceiveFailure|无法从远程服务器接收完整的响应。|  
|RequestCanceled|请求已被取消。|  
|SecureChannelFailure|安全通道链接中出现错误。|  
|SendFailure|无法向远程服务器发送完整的请求。|  
|ServerProtocolViolation|服务器响应不是有效的 HTTP 响应。|  
|成功|未遇到任何错误。|  
|超时|在请求的超时设置内未接收到任何响应。|  
|TrustFailure|无法验证服务器证书。|  
|MessageLengthLimitExceeded|从服务器发送请求或接收响应时，接收到的消息超出指定限制。|  
|挂起|内部异步请求处于挂起状态。|  
|PipelineFailure|此值支持 .NET Framework 基础结构，不能在代码中直接使用。|  
|ProxyNameResolutionFailure|名称解析程序服务无法解析代理主机名。|  
|UnknownError|出现未知类型的异常。|  
  
当“状态”属性为“WebExceptionStatus.ProtocolError”时，可以使用包含来自服务器的响应的 WebResponse  。 可以检查此响应以确定协议错误的实际来源。  
  
下面的示例演示如何捕获 WebException。  
  
```csharp  
try
{  
    // Create a request instance.  
    WebRequest myRequest =
    WebRequest.Create("http://www.contoso.com");  
    // Get the response.  
    WebResponse myResponse = myRequest.GetResponse();  
    //Get a readable stream from the server.
    Stream sr = myResponse.GetResponseStream();  
  
    //Read from the stream and write any data to the console.  
    bytesread = sr.Read( myBuffer, 0, length);  
    while( bytesread > 0 )
    {  
        for (int i=0; i<bytesread; i++) {  
            Console.Write( "{0}", myBuffer[i]);  
        }  
        Console.WriteLine();  
        bytesread = sr.Read( myBuffer, 0, length);  
    }  
    sr.Close();  
    myResponse.Close();  
}  
catch (WebException webExcp)
{  
    // If you reach this point, an exception has been caught.  
    Console.WriteLine("A WebException has been caught.");  
    // Write out the WebException message.  
    Console.WriteLine(webExcp.ToString());  
    // Get the WebException status code.  
    WebExceptionStatus status =  webExcp.Status;  
    // If status is WebExceptionStatus.ProtocolError,
    //   there has been a protocol error and a WebResponse
    //   should exist. Display the protocol error.  
    if (status == WebExceptionStatus.ProtocolError) {  
        Console.Write("The server returned protocol error ");  
        // Get HttpWebResponse so that you can check the HTTP status code.  
        HttpWebResponse httpResponse = (HttpWebResponse)webExcp.Response;  
        Console.WriteLine((int)httpResponse.StatusCode + " - "  
           + httpResponse.StatusCode);  
    }  
}  
catch (Exception e)
{  
    // Code to catch other exceptions goes here.  
}  
```  
  
```vb  
Try  
    ' Create a request instance.  
    Dim myRequest As WebRequest = WebRequest.Create("http://www.contoso.com")  
    ' Get the response.  
    Dim myResponse As WebResponse = myRequest.GetResponse()  
    'Get a readable stream from the server.
    Dim sr As Stream = myResponse.GetResponseStream()  
  
    Dim i As Integer
    'Read from the stream and write any data to the console.  
    bytesread = sr.Read(myBuffer, 0, length)  
    While bytesread > 0  
        For i = 0 To bytesread - 1  
            Console.Write("{0}", myBuffer(i))  
        Next i  
        Console.WriteLine()  
        bytesread = sr.Read(myBuffer, 0, length)  
    End While  
    sr.Close()  
    myResponse.Close()  
Catch webExcp As WebException  
    ' If you reach this point, an exception has been caught.  
    Console.WriteLine("A WebException has been caught.")  
    ' Write out the WebException message.  
    Console.WriteLine(webExcp.ToString())  
    ' Get the WebException status code.  
    Dim status As WebExceptionStatus = webExcp.Status  
    ' If status is WebExceptionStatus.ProtocolError,
    '   there has been a protocol error and a WebResponse
    '   should exist. Display the protocol error.  
    If status = WebExceptionStatus.ProtocolError Then  
        Console.Write("The server returned protocol error ")  
        ' Get HttpWebResponse so that you can check the HTTP status code.  
        Dim httpResponse As HttpWebResponse = _  
           CType(webExcp.Response, HttpWebResponse)  
        Console.WriteLine(CInt(httpResponse.StatusCode).ToString() & _  
           " - " & httpResponse.StatusCode.ToString())  
    End If  
Catch e As Exception  
    ' Code to catch other exceptions goes here.  
End Try  
```  
  
当 Windows 套接字上出现错误时，使用 <xref:System.Net.Sockets.Socket> 类的应用程序将引发 <xref:System.Net.Sockets.SocketException>。 <xref:System.Net.Sockets.TcpClient>、<xref:System.Net.Sockets.TcpListener> 和 <xref:System.Net.Sockets.UdpClient> 类在套接字类的顶层生成，并且也会引发 SocketExceptions 。  
  
引发 SocketException 时，SocketException 类将 <xref:System.Net.Sockets.SocketException.ErrorCode%2A> 属性设置为最后一次发生的操作系统套接字错误 。 有关套接字错误代码的详细信息，请参阅 MSDN 中的 Winsock 2.0 API 错误代码文档。  
  
## <a name="see-also"></a>请参阅

- [在 .NET 中处理和引发异常](../../standard/exceptions/index.md)
- [请求数据](requesting-data.md)
