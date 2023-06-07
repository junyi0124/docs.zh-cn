---
title: 从 WebResponse 派生
ms.date: 03/30/2017
helpviewer_keywords:
- Deriving from WebResponse
ms.assetid: f11d4866-a199-4087-9306-a5a4c18b13db
ms.openlocfilehash: 793533b632952df3f0866bb6377efd0e313cd007
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287522"
---
# <a name="deriving-from-webresponse"></a>从 WebResponse 派生

<xref:System.Net.WebResponse> 类是一个抽象基类，可为创建适合 .NET Framework 可插入协议模型的协议特定的响应提供基本方法和属性。 使用 <xref:System.Net.WebRequest> 类从资源请求数据的应用程序会在 WebResponse 中接收响应  。 协议特定的 WebResponse 后代必须实现 WebResponse 类的抽象成员   。  
  
 关联的 WebRequest 类必须创建 WebResponse 后代   。 例如，<xref:System.Net.HttpWebResponse> 实例仅作为调用 <xref:System.Net.HttpWebRequest.GetResponse%2A?displayProperty=nameWithType> 或 <xref:System.Net.HttpWebRequest.EndGetResponse%2A?displayProperty=nameWithType> 的结果而创建。 每个 WebResponse 都包含对资源的请求结果，不可重复使用  。  
  
## <a name="contentlength-property"></a>ContentLength 属性  

 <xref:System.Net.WebResponse.ContentLength%2A> 属性表示从 <xref:System.Net.WebResponse.GetResponseStream%2A> 方法返回的流中可用的数据字节数。 ContentLength 属性不表示服务器返回的标头或元数据信息的字节数，仅表示所请求资源本身的数据字节数  。  
  
## <a name="contenttype-property"></a>ContentType 属性  

 <xref:System.Net.WebResponse.ContentType%2A> 属性提供协议要求用户发送至客户端的所有特殊信息，以确定服务器发送的内容的类型。 这通常是所有返回数据的 MIME 内容类型。  
  
## <a name="headers-property"></a>Headers 属性  

 <xref:System.Net.WebResponse.Headers%2A> 属性包含与响应相关联的元数据名称/值对的任意集合。 协议所需的任何可表示为名称/值对的元数据都可以包含在 Headers 属性中  。  
  
 用户无需使用 Headers 属性即可使用标头元数据  。 协议特定的元数据可作为属性公开，例如，<xref:System.Net.HttpWebResponse.LastModified%2A?displayProperty=nameWithType> 属性公开了 Last-Modified HTTP 标头  。 将标头元数据作为属性公开时，应禁止使用 Headers 属性设置相同的属性  。  
  
## <a name="responseuri-property"></a>ResponseUri 属性  

 <xref:System.Net.WebResponse.ResponseUri%2A> 属性包含实际提供响应的资源的 URI。 对于不支持重定向的协议，ResponseUri 与创建响应的 WebRequest 的 <xref:System.Net.WebRequest.RequestUri%2A> 属性相同。 如果协议支持重定向请求，则 ResponseUri 将包含响应的 URI  。  
  
## <a name="close-method"></a>Close 方法  

 <xref:System.Net.WebResponse.Close%2A> 方法关闭请求和响应进行的任何连接，并清除响应使用的资源。 Close 方法关闭响应使用的所有流实例，但如果之前通过调用 <xref:System.IO.Stream.Close%2A?displayProperty=nameWithType> 方法关闭了响应流，也不会引发异常。  
  
## <a name="getresponsestream-method"></a>GetResponseStream 方法  

 <xref:System.Net.WebResponse.GetResponseStream%2A> 方法返回的流中包含所请求资源的响应。 响应流仅包含资源返回的数据，响应中包含的任何标头或元数据应从响应中删除，并通过协议特定的属性或 Headers 属性公开给应用程序  。  
  
 GetResponseStream 方法返回的流实例为应用程序所有，无需关闭 WebResponse 即可将其关闭   。 按照惯例，调用 WebResponse.Close 方法也会关闭 GetResponse 返回的流   。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.WebResponse>
- <xref:System.Net.HttpWebResponse>
- <xref:System.Net.FileWebResponse>
- [对可插入协议进行编程](programming-pluggable-protocols.md)
- [从 WebRequest 派生](deriving-from-webrequest.md)
