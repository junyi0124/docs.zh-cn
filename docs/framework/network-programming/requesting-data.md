---
title: 正在请求数据...
description: 了解如何通过可插入协议开发使用单一接口从多个协议检索数据的应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sending data
- WebRequest class, sending and receiving data
- requesting data from Internet, about requesting data
- WebClient class, sending and receiving data
- network, requesting data
- receiving data
- sending data, about sending data
- response to Internet request, about responding to Internet requests
- data requests
- receiving data, about receiving data
- Internet, requesting data
ms.assetid: df6f1e1d-6f2a-45dd-8141-4a85c3dafe1d
ms.openlocfilehash: 87ad0144f57bdca0e0235aea30c4ab450cc890f4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279293"
---
# <a name="requesting-data"></a>正在请求数据...

开发在如今 Internet 的分布式操作环境中运行的应用程序需要使用高效易用的方法从所有类型的资源中检索数据。 可以通过可插入协议开发使用单一接口从多个 Internet 协议检索数据的应用程序。  
  
## <a name="uploading-and-downloading-data-from-an-internet-server"></a>从 Internet 服务器上传和下载数据  

 对于简单的请求和响应事务，<xref:System.Net.WebClient> 类提供最简单的方法来将数据上传到 Internet 服务器或从 Internet 服务器下载数据。 WebClient 提供用于上传和下载文件、发送和接收数据流和将数据缓冲区发送到服务器并接收响应的方法。 WebClient 使用 <xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse> 类来实现与 Internet 资源的实际连接，因此可以使用任何已注册的可插入协议。  
  
 需要执行更复杂事务的客户端应用程序使用 WebRequest 类及其后代从服务器请求数据。 WebRequest 封装连接到服务器、发送请求和接收响应的详细信息。 WebRequest 是一个抽象类，它定义一组使用可插入协议的所有应用程序均可使用的属性和方法。 WebRequest 的后代（如 <xref:System.Net.HttpWebRequest>）通过与基础协议一致的方法实现由 WebRequest 定义的属性和方法 。  
  
 WebRequest 类使用传递到 <xref:System.Net.WebRequest.Create%2A> 方法的 URI 值确定要创建的特定派生类实例，创建 WebRequest 后代特定于协议的实例 。 应用程序通过 <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> 方法注册 WebRequest 后代的构造函数，指示将用于处理请求的后代。  
  
 通过调用 WebRequest 上的 <xref:System.Net.WebRequest.GetResponse%2A> 方法，向 Internet 资源发送请求。 GetResponse 方法从 WebRequest 的属性构造特定于协议的请求、将 TCP 或 UDP 套接字连接到服务器并发送请求 。 对于将数据发送到服务器的请求（如 HTTP Post 或 FTP Put 请求），<xref:System.Net.WebRequest.GetRequestStream%2A?displayProperty=nameWithType> 方法提供要向其发送数据的网络数据流 。  
  
 GetResponse 方法返回特定于协议的 WebResponse，后者与 WebRequest 匹配  。  
  
 WebResponse 类也是一个抽象类，它定义使用可插入协议的所有应用程序均可使用的属性和方法。 WebResponse 后代为基础协议实现这些属性和方法。 例如，<xref:System.Net.HttpWebResponse> 类为 HTTP 实现 WebResponse 类。  
  
 向由 <xref:System.Net.WebResponse.GetResponseStream%2A?displayProperty=nameWithType> 方法返回的数据流中的应用程序提供服务器返回的数据。 此数据流的用法与任何其他数据流相似，如下面的示例所示。  
  
```csharp  
StreamReader sr =  
   new StreamReader(resp.GetResponseStream(), Encoding.ASCII);  
```  
  
```vb  
Dim sr As StreamReader  
sr = New StreamReader(resp.GetResponseStream(), Encoding.ASCII)  
```  
  
## <a name="see-also"></a>请参阅

- [.NET Framework 中的网络编程](index.md)
- [如何：请求网页并以数据流的形式检索结果](how-to-request-a-web-page-and-retrieve-the-results-as-a-stream.md)
- [如何：检索与 WebRequest 匹配的特定于协议的 WebResponse](how-to-retrieve-a-protocol-specific-webresponse-that-matches-a-webrequest.md)
