---
title: 在网络上使用流
description: .NET Framework 将网络资源表示为流。 NetworkStream 类实现与网络资源一起使用的 Stream 类。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- requesting data from Internet, streams
- Networking
- response to Internet request, streams
- network resources, stream capabilities
- receiving data, stream capabilities
- Network Resources
- sending data, stream capabilities
- downloading Internet resources, streams
- streams, capabilities
- Internet, streams
- streams
ms.assetid: 02b05fba-7235-45ce-94e5-060436ee0875
ms.openlocfilehash: c59e4aa2edad7b28203cfce5f568f8ccb8558dbb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263108"
---
# <a name="using-streams-on-the-network"></a>在网络上使用流

网络资源在 .NET Framework 中表示为流。 通过对流进行一般处理，.NET Framework 提供下列功能：  
  
- 发送和接收 Web 数据的通用方法。 无论文件的实际内容是什么（HTML、XML 或任何其他内容），应用程序都将使用 <xref:System.IO.Stream.Write%2A?displayProperty=nameWithType> 和 <xref:System.IO.Stream.Read%2A?displayProperty=nameWithType> 发送和接收数据。  
  
- 跨 .NET Framework 的流兼容性。 流在 .NET Framework 中遍及使用，其具有丰富的基础结构来处理流。 例如，通过仅更改初始化流的几行代码，便可修改从 <xref:System.IO.FileStream> 中读取 XML 数据的应用程序而使其改为从 <xref:System.Net.Sockets.NetworkStream> 中读取数据。 NetworkStream 类和其他流之间的主要区别在于：NetworkStream 是不可查找的，<xref:System.Net.Sockets.NetworkStream.CanSeek%2A> 属性始终返回 false，且 <xref:System.Net.Sockets.NetworkStream.Seek%2A> 和 <xref:System.Net.Sockets.NetworkStream.Position%2A> 方法将引发 <xref:System.NotSupportedException>。    
  
- 在数据到达时处理数据。 流在数据从网络到达目标时便提供对数据的访问，而不会强制应用程序等待整个数据集下载完成。  
  
 <xref:System.Net.Sockets> 命名空间包含一个 NetworkStream 类，该类实现专用于网络资源的 <xref:System.IO.Stream> 类。 <xref:System.Net.Sockets> 命名空间中的类使用 NetworkStream 类表示流。  
  
 若要使用返回的流向网络发送数据，请对 <xref:System.Net.WebRequest> 调用 <xref:System.Net.WebRequest.GetRequestStream%2A>。 WebRequest 会将请求标头发送到服务器；然后你可以通过对返回的流调用 <xref:System.IO.Stream.BeginWrite%2A>、<xref:System.IO.Stream.EndWrite%2A> 或 <xref:System.IO.Stream.Write%2A> 方法，将数据发送到网络资源。 某些协议（如 HTTP）可能要求在发送数据之前设置协议特定的属性。 下面的代码示例演示如何设置 HTTP 特定的属性以发送数据。 该示例假定变量 `sendData` 包含要发送的数据，变量 `sendLength` 为要发送的数据的字节数。  
  
```csharp  
HttpWebRequest request =
   (HttpWebRequest) WebRequest.Create("http://www.contoso.com/");  
request.Method = "POST";  
request.ContentLength = sendLength;  
try  
{  
   Stream sendStream = request.GetRequestStream();  
   sendStream.Write(sendData,0,sendLength);  
   sendStream.Close();  
}  
catch  
{  
   // Handle errors . . .  
}  
```  
  
```vb  
Dim request As HttpWebRequest = _  
   CType(WebRequest.Create("http://www.contoso.com/"), HttpWebRequest)  
request.Method = "POST"  
request.ContentLength = sendLength  
Try  
   Dim sendStream As Stream = request.GetRequestStream()  
   sendStream.Write(sendData, 0, sendLength)  
   sendStream.Close()  
Catch  
   ' Handle errors . . .  
End Try  
```  
  
 若要从网络接收数据，请对 <xref:System.Net.WebResponse> 调用 <xref:System.Net.WebResponse.GetResponseStream%2A>。 然后可以通过对返回的流调用 <xref:System.IO.Stream.BeginRead%2A>、<xref:System.IO.Stream.EndRead%2A> 或 <xref:System.IO.Stream.Read%2A> 方法，从网络资源读取数据。  
  
 使用来自网络资源的流时，请留心以下几点：  
  
- CanSeek 属性将始终返回 false，因为 NetworkStream 类无法更改流中的位置。   Seek 和 Position 方法引发 NotSupportedException。    
  
- 当使用 WebRequest 和 WebResponse 时，通过调用 GetResponseStream 创建的流实例是只读的，而通过调用 GetRequestStream 创建的流实例是只写的。     
  
- 使用 <xref:System.IO.StreamReader> 类可使编码更容易。 下面的代码示例使用 StreamReader 从 WebResponse 读取用 ASCII 编码的流（此示例不显示创建请求的过程）。   
  
- 如果网络资源不可用，则对 GetResponse 的调用可能受到阻止。 应考虑利用 <xref:System.Net.WebRequest.BeginGetResponse%2A> 和 <xref:System.Net.WebRequest.EndGetResponse%2A> 方法使用异步请求。  
  
- 在创建与服务器的连接时，对 GetRequestStream 的调用可能受到阻止。 应考虑利用 <xref:System.Net.WebRequest.BeginGetRequestStream%2A> 和 <xref:System.Net.WebRequest.EndGetRequestStream%2A> 方法对流使用异步请求。  
  
```csharp  
// Create a response object.  
WebResponse response = request.GetResponse();  
// Get a readable stream from the server.  
StreamReader sr =
   new StreamReader(response.GetResponseStream(), Encoding.ASCII);  
// Use the stream. Remember when you are through with the stream to close it.  
sr.Close();  
```  
  
```vb  
' Create a response object.  
Dim response As WebResponse = request.GetResponse()  
' Get a readable stream from the server.  
Dim sr As _
   New StreamReader(response.GetResponseStream(), Encoding.ASCII)  
' Use the stream. Remember when you are through with the stream to close it.  
sr.Close()  
```  
  
## <a name="see-also"></a>请参阅

- [如何：使用 WebRequest 类请求数据](how-to-request-data-using-the-webrequest-class.md)
- [请求数据](requesting-data.md)
