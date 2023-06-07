---
title: 从 WebRequest 派生
ms.date: 03/30/2017
helpviewer_keywords:
- WebRequest class, pluggable protocols
- protocol-specific request handler
- sending data, pluggable protocols
- pluggable protocols, class criteria
- Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
ms.assetid: 9810c177-973e-43d7-823c-14960bd625ea
ms.openlocfilehash: a480f38aeefed3481187e5027409a49d1535c9f4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250549"
---
# <a name="deriving-from-webrequest"></a>从 WebRequest 派生

<xref:System.Net.WebRequest> 类是一个抽象基类，可为创建适合 .NET Framework 可插入协议模型的协议特定的请求处理程序提供基本方法和属性。 使用 WebRequest 类的应用程序可使用任何支持的协议请求数据，而无需指定所使用的协议  。  
  
 必须满足两个条件才能将协议特定的类用作可插入协议：该类必须实现 <xref:System.Net.IWebRequestCreate> 接口，并且必须使用 <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> 方法注册。 该类必须覆盖 WebRequest 的所有抽象方法和属性，才能提供可插入接口  。  
  
 WebRequest 实例是一次性的，如果想再发出请求，请创建一个新的 WebRequest   。 WebRequest 支持 <xref:System.Runtime.Serialization.ISerializable> 接口，使开发人员能够序列化模板 WebRequest，然后针对其他请求重新构建模板。  
  
## <a name="iwebrequest-create-method"></a>IWebRequest Create 方法  

 <xref:System.Net.IWebRequestCreate.Create%2A> 方法负责初始化协议特定的类的新实例。 创建新 WebRequest 后，<xref:System.Net.WebRequest.Create%2A?displayProperty=nameWithType> 方法会将请求的 URI 与使用 RegisterPrefix 方法注册的 URI 前缀进行匹配。 正确的协议特定后代的 Create 方法必须返回该后代的初始化实例，该实例能够为协议执行标准请求/响应事务，且不需要修改任何协议特定的字段  。  
  
## <a name="connectiongroupname-property"></a>ConnectionGroupName 属性  

 <xref:System.Net.WebRequest.ConnectionGroupName%2A> 属性用于命名一组资源连接，以便可通过单个连接发出多个请求。 若要实现连接共享，必须使用协议特定的方法来集中和分配连接。 例如，提供的 <xref:System.Net.ServicePointManager> 类实现 <xref:System.Net.HttpWebRequest> 类的连接共享。 ServicePointManager 类创建可为每个连接组提供特定服务器连接的 <xref:System.Net.ServicePoint>。  
  
## <a name="contentlength-property"></a>ContentLength 属性  

 <xref:System.Net.WebRequest.ContentLength%2A> 属性指定上传数据时将发送到服务器的数据的字节数。  
  
 通常，当 ContentLength 属性设置为大于零的值时，必须将 <xref:System.Net.WebRequest.Method%2A> 属性设置为表示正在上传  。  
  
## <a name="contenttype-property"></a>ContentType 属性  

 <xref:System.Net.WebRequest.ContentType%2A> 属性提供协议要求用户发送至服务器的所有特殊信息，以确定发送的内容的类型。 这通常是所有上传数据的 MIME 内容类型。  
  
## <a name="credentials-property"></a>Credentials 属性  

 <xref:System.Net.WebRequest.Credentials%2A> 属性包含服务器对请求进行身份验证所需的信息。 用户必须实现协议的身份验证过程的详细信息。 <xref:System.Net.AuthenticationManager> 类负责对请求进行身份验证和提供身份验证令牌。 提供协议使用的凭据的类必须实现 <xref:System.Net.ICredentials> 接口。  
  
## <a name="headers-property"></a>Headers 属性  

 <xref:System.Net.WebRequest.Headers%2A> 属性包含与请求相关联的元数据的名称/值对的任意集合。 协议所需的任何可表示为名称/值对的元数据都可以包含在 Headers 属性中  。 通常，在调用 <xref:System.Net.WebRequest.GetRequestStream%2A> 或 <xref:System.Net.WebRequest.GetResponse%2A> 方法之前必须先设置此信息；一旦发出请求，元数据就会被视为只读。  
  
 用户无需使用 Headers 属性即可使用标头元数据  。 协议特定的元数据可作为属性公开，例如，<xref:System.Net.HttpWebRequest.UserAgent%2A?displayProperty=nameWithType> 属性公开了 User-Agent HTTP 标头  。 将标头元数据作为属性公开时，应禁止使用 Headers 属性设置相同的属性  。  
  
## <a name="method-property"></a>Method 属性  

 <xref:System.Net.WebRequest.Method%2A> 属性包含请求要求服务器执行的动作或操作。 Method 属性的默认值必须启用标准请求/响应操作，无需设置任何协议特定的属性  。 例如，<xref:System.Net.HttpWebResponse.Method%2A> 方法默认为 GET，该方法从 Web 服务器请求资源并返回响应。  
  
 通常，当 Method 属性设置为表明正在进行上传的动作或操作时，ContentLength 属性的值必须大于零   。  
  
## <a name="preauthenticate-property"></a>PreAuthenticate 属性  

 应用程序设置 <xref:System.Net.WebRequest.PreAuthenticate%2A> 属性以指示应随初始请求一起发送身份验证信息，而不应等待身份验证质询。 仅当协议支持随初始请求一起发送的身份验证凭据时，PreAuthenticate 属性才有意义  。  
  
## <a name="proxy-property"></a>Proxy 属性  

 <xref:System.Net.WebRequest.Proxy%2A> 属性包含用于访问所请求资源的 <xref:System.Net.IWebProxy> 接口。 仅当协议支持代理请求时，Proxy 属性才有意义  。 如果协议需要代理，则必须设置默认代理。  
  
 在某些环境中，例如公司防火墙后，可能会要求协议使用代理。 在这种情况下，必须实现 IWebProxy 接口以创建可用于协议的代理类  。  
  
## <a name="requesturi-property"></a>RequestUri 属性  

 <xref:System.Net.WebRequest.RequestUri%2A> 属性包含传递给 WebRequest.Create 方法的 URI  。 创建 WebRequest 后，它就为只读且不能更改  。 如果协议支持重定向，则可由其他 URI 所标识的资源发出响应。 如需提供对响应 URI 的访问权限，则必须提供一个包含该 URI 的其他属性。  
  
## <a name="timeout-property"></a>超时属性  

 <xref:System.Net.WebRequest.Timeout%2A> 属性包含请求超时并引发异常前等待的时长（以毫秒为单位）。 Timeout 仅适用于使用 <xref:System.Net.WebRequest.GetResponse%2A> 方法发出的同步请求，异步请求必须使用 <xref:System.Net.WebRequest.Abort%2A> 方法取消挂起的请求。  
  
 仅当协议特定的类实现超时过程时，设置 Timeout 属性才有意义  。  
  
## <a name="abort-method"></a>Abort 方法  

 <xref:System.Net.WebRequest.Abort%2A> 方法取消服务器的挂起异步请求。 取消请求后，调用 GetResponse、BeginGetResponse、EndGetResponse、GetRequestStream、BeginGetRequestStream 或 EndGetRequestStream 将引发 <xref:System.Net.WebException>，且<xref:System.Net.WebException.Status%2A> 属性被设置为 <xref:System.Net.WebExceptionStatus>。  
  
## <a name="begingetrequeststream-and-endgetrequeststream-methods"></a>BeginGetRequestStream 和 EndGetRequestStream 方法  

 <xref:System.Net.WebRequest.BeginGetRequestStream%2A> 方法为用于将数据上传到服务器的流启动异步请求。 <xref:System.Net.WebRequest.EndGetRequestStream%2A> 方法完成异步请求并返回所请求的流。 这些方法使用标准 .NET Framework 异步模式实现 GetRequestStream 方法  。  
  
## <a name="begingetresponse-and-endgetresponse-methods"></a>BeginGetResponse 和 EndGetResponse 方法  

 <xref:System.Net.WebRequest.BeginGetResponse%2A> 方法启动服务器的异步请求。 <xref:System.Net.WebRequest.EndGetResponse%2A> 方法完成异步请求并返回请求的响应。 这些方法使用标准 .NET Framework 异步模式实现 GetResponse 方法  。  
  
## <a name="getrequeststream-method"></a>GetRequestStream 方法  

 <xref:System.Net.WebRequest.GetRequestStream%2A> 方法返回用于将数据写入请求的服务器的流。 返回的流应为只写流，不会进行查找，它作为写入服务器的单向数据流。 该流针对 <xref:System.IO.Stream.CanRead%2A> 和 <xref:System.IO.Stream.CanSeek%2A> 属性返回 false，针对 <xref:System.IO.Stream.CanWrite%2A> 属性返回 true。  
  
 返回流之前，GetRequestStream 方法通常会与服务器建立连接，并发送指示正在将数据发送到服务器的标头信息  。 由于 GetRequestStream 开始请求，因此在调用 GetRequestStream 后，通常不允许设置任何 Header 属性或 ContentLength 属性     。  
  
## <a name="getresponse-method"></a>GetResponse 方法  

 <xref:System.Net.WebRequest.GetResponse%2A> 方法返回 <xref:System.Net.WebResponse> 类的协议特定的后代，表示来自服务器的响应。 除非请求已经由 GetRequestStream 方法启动，否则 GetResponse 方法会与 RequestUri 标识的资源建立连接，发送标头信息，指示发出的请求的类型，然后从资源接收响应    。  
  
 调用 GetResponse 方法后，所有属性都应该被视为只读  。 WebRequest 实例是一次性的；如果想再发出请求，应创建一个新的 WebRequest   。  
  
 GetResponse 方法负责创建适当的 WebResponse 后代来包含传入的响应   。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.WebRequest>
- <xref:System.Net.HttpWebRequest>
- <xref:System.Net.FileWebRequest>
- [对可插入协议进行编程](programming-pluggable-protocols.md)
- [从 WebResponse 派生](deriving-from-webresponse.md)
