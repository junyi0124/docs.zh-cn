---
title: WCF 疑难解答快速入门
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], troubleshooting
- Windows Communication Foundation [WCF], troubleshooting
ms.assetid: a9ea7a53-f31a-46eb-806e-898e465a4992
ms.openlocfilehash: 7df178334ca3ef5b901e3e82bf39592434ee059e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279722"
---
# <a name="wcf-troubleshooting-quickstart"></a>WCF 疑难解答快速入门

本主题列出了一些客户开发 WCF 客户端和服务时所遇到的已知问题。 如果您遇到的问题不在此列表中，我们建议您为您的服务配置跟踪。 这将生成一个跟踪文件，您可以使用跟踪文件查看器查看它并获取有关服务中可能发生的异常的详细信息。 有关配置跟踪的详细信息，请参阅： [Configuring Tracing](./diagnostics/tracing/configuring-tracing.md)。 有关跟踪文件查看器的详细信息，请参阅： [Service Trace Viewer Tool (SvcTraceViewer.exe)](service-trace-viewer-tool-svctraceviewer-exe.md)。  
  
1. [在安装 Windows 7 和 IIS 后，当我尝试浏览到 WCF 服务时，收到以下错误消息：HTTP 错误 404.3 – 找不到](#bkmk_0)  
  
     HTTP 错误 404.3 – 找不到：由于扩展配置，无法提供您请求的页面。 如果此页是脚本，请添加处理程序。 如果应下载文件，请添加 MIME 映射。 详细错误 InformationModule StaticFileModule。  
  
2. [如果客户端在第一次请求之后空闲一段时间，则在第二次请求时，会收到 MessageSecurityException。发生了什么事情？](#BKMK_q1)  
  
3. [在大约10个客户端与客户端交互后，我的服务开始拒绝新的客户端。发生了什么事情？](#BKMK_q2)  
  
4. [我是否可以从 WCF 应用程序的配置文件以外的某处加载我的服务配置？](#BKMK_q3)  
  
5. [我的服务和客户端工作得很好，但当客户端在另一台计算机上时，我无法使其正常工作？发生了什么事情？](#BKMK_q4)  
  
6. [当我引发 \<Exception> 类型为异常的 FaultException 时，始终会在客户端上收到常规 FaultException 类型，而不是泛型类型。发生了什么事情？](#BKMK_q5)  
  
7. [当回复不包含任何数据时，与单向和请求-答复操作的返回速度几乎相同。发生了什么事情？](#BKMK_q6)  
  
8. [我将 x.509 证书与我的服务一起使用，并获得 System.security.cryptography.cryptographicexception。发生了什么事情？](#BKMK_q77)  
  
9. [我将操作的第一个参数从大写改为小写;现在我的客户端引发异常。发生了什么事情？](#BKMK_q88)  
  
10. [我使用的是我的跟踪工具之一，并获得 System.servicemodel.endpointnotfoundexception。发生了什么事情？](#BKMK_q99)  
  
11. [从 WCF SOAP 应用程序调用 WCF Web HTTP 应用程序时，服务返回以下错误：405 不允许的方法](#BK_MK99)  
  
 [什么是基址？它如何与终结点地址相关联？](#BKMK_q10)  
  
<a name="bkmk_0"></a>

## <a name="after-installing-windows-7-and-iis-when-i-attempt-to-browse-to-a-wcf-service-i-get-the-following-error-message-http-error-4043--not-found"></a>在安装 Windows 7 和 IIS 后，当我尝试浏览到 WCF 服务时，收到以下错误消息：HTTP 错误 404.3 – 找不到  

 完整错误消息为：  
  
 HTTP 错误 404.3 – 找不到：由于扩展配置，无法提供您请求的页面。 如果此页是脚本，请添加处理程序。 如果应下载文件，请添加 MIME 映射。 详细错误 InformationModule StaticFileModule。  
  
 当未在控制面板中显式设置 "Windows Communication Foundation HTTP 激活" 时，会出现此错误消息。 若要设置此项，请在窗口的左下角单击 "程序"。 单击“打开或关闭 Windows 功能”。 展开 Microsoft .NET Framework 3.5.1，选中“Windows Communication Foundation HTTP 激活”。  
  
<a name="BKMK_q1"></a>

## <a name="sometimes-i-receive-a-messagesecurityexception-on-the-second-request-if-my-client-is-idle-for-a-while-after-the-first-request-what-is-happening"></a>如果我的客户端在第一次请求后暂时处于空闲，有时我会在第二次请求时收到 MessageSecurityException。 发生了什么？  

 第二次请求失败主要有两个原因：(1) 会话已超时或 (2) 承载服务的 Web 服务器被回收。 在第一种情况下，会话始终有效，直到服务超时。如果服务在服务的绑定 () 中指定的时间段内未收到来自客户端的请求 <xref:System.ServiceModel.Channels.Binding.ReceiveTimeout%2A> ，则该服务将终止安全会话。 后续客户端消息会导致 <xref:System.ServiceModel.Security.MessageSecurityException>。 客户端必须重新建立与服务的安全会话，才能发送以后的消息或使用有状态安全上下文令牌。 有状态的安全上下文令牌还允许安全会话在回收 Web 服务器后存在。 有关在安全会话中使用有状态安全上下文令牌的详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](./feature-details/how-to-create-a-security-context-token-for-a-secure-session.md)。 或者，也可禁用安全会话。 使用 [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) 绑定时，可以将 `establishSecurityContext` 属性设置为 `false` 以禁用安全会话。 若要为其他绑定禁用安全会话，必须创建自定义绑定。 有关创建自定义绑定的详细信息，请参阅 [How to: Create a Custom Binding Using the SecurityBindingElement](./feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)。 在应用任何这些选择前，您必须先了解应用程序的安全要求。  
  
<a name="BKMK_q2"></a>

## <a name="my-service-starts-to-reject-new-clients-after-about-10-clients-are-interacting-with-it-what-is-happening"></a>大约有 10 个客户端与我的服务交互后，我的服务开始拒绝新的客户端。 发生了什么？  

 默认情况下，服务最多只能有 10 个并发会话。 因此，如果服务绑定使用会话，则服务将接受新的客户端连接，直到到达该数目；之后，它将拒绝新的客户端连接，直到当前会话之一结束。 可以通过多种方式支持更多的客户端。 如果你的服务不要求会话，则不要使用会话绑定。  (有关详细信息，请参阅 [使用会话](using-sessions.md)。 ) 另一个选项是通过将属性的值更改 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentSessions%2A> 为适合你的情况的数字来增加会话限制。  
  
<a name="BKMK_q3"></a>

## <a name="can-i-load-my-service-configuration-from-somewhere-other-than-the-wcf-applications-configuration-file"></a>我是否可以从 WCF 应用程序的配置文件以外的某处加载我的服务配置？  

 是。不过，您必须创建自定义 <xref:System.ServiceModel.ServiceHost> 类，并重写 <xref:System.ServiceModel.ServiceHostBase.ApplyConfiguration%2A> 方法。 在此方法内，您可以调用 base 先加载配置（如果您还要加载标准配置信息），但是也可以整个替换配置加载系统。 如果要从不同于应用程序配置文件的配置文件中加载配置，则必须自行分析配置文件，然后加载配置。  
  
 下面的代码示例演示如何重写 <xref:System.ServiceModel.ServiceHostBase.ApplyConfiguration%2A> 方法以及直接配置终结点。  
  
```csharp
public class MyServiceHost : ServiceHost  
{  
    public MyServiceHost(Type serviceType, params Uri[] baseAddresses)
      : base(serviceType, baseAddresses)  
    {
        Console.WriteLine("MyServiceHost Constructor");
    }  
  
    protected override void ApplyConfiguration()  
    {  
        string straddress = GetAddress();  
        Uri address = new Uri(straddress);  
        Binding binding = GetBinding();  
        base.AddServiceEndpoint(typeof(IData), binding, address);  
    }  
  
    string GetAddress()  
    {
        return "http://MyMachine:7777/MyEndpointAddress/";
    }  
  
    Binding GetBinding()  
    {  
        WSHttpBinding binding = new WSHttpBinding();  
        binding.Security.Mode = SecurityMode.None;  
        return binding;  
    }  
}  
```  
  
<a name="BKMK_q4"></a>

## <a name="my-service-and-client-work-great-but-i-cant-get-them-to-work-when-the-client-is-on-another-computer-whats-happening"></a>我的服务和客户端工作得很好，但是当客户端位于另一台计算机上时，为何就无法正常工作？ 发生了什么情况？  

 根据异常的不同，可能会有多个问题：  
  
- 可能需要将客户端终结点的地址更改为主机名，而不是“localhost”。  
  
- 可能需要打开应用程序的端口。 有关详细信息，请参阅 SDK 示例中的 [Firewall Instructions](./samples/firewall-instructions.md) 。  
  
- 有关其他可能的问题，请参阅示例主题 [运行 Windows Communication Foundation 示例](./samples/running-the-samples.md)。  
  
- 如果客户端使用的是 Windows 凭据并且异常为 <xref:System.ServiceModel.Security.SecurityNegotiationException>，则按如下方式配置 Kerberos。  
  
    1. 将标识凭据添加到客户端的 App.config 文件中的终结点元素：  
  
        ```xml
        <endpoint
          address="http://MyServer:8000/MyService/"
          binding="wsHttpBinding"
          bindingConfiguration="WSHttpBinding_IServiceExample"
          contract="IServiceExample"
          behaviorConfiguration="ClientCredBehavior"
          name="WSHttpBinding_IServiceExample">  
          <identity>  
            <userPrincipalName value="name@corp.contoso.com"/>  
          </identity>  
        </endpoint>  
        ```  
  
    2. 在 System 或 NetworkService 帐户下运行自承载服务。 可以运行此命令在 System 帐户下创建命令窗口：  
  
        ```console
        at 12:36 /interactive "cmd.exe"  
        ```  
  
    3. 在 Internet 信息服务 (IIS) 下承载服务，默认情况下，IIS 使用服务主体名称 (SPN) 帐户。  
  
    4. 使用 SetSPN 在域中注册一个新的 SPN。 你需要是域管理员才能执行此操作。  
  
 有关 Kerberos 协议的详细信息，请参阅 [WCF 中使用的安全概念](./feature-details/security-concepts-used-in-wcf.md) 和：  
  
- [调试 Windows 身份验证错误](./feature-details/debugging-windows-authentication-errors.md)  
  
- [Registering Kerberos Service Principal Names by Using Http.sys（使用 Http.sys 注册 Kerberos 服务主体名称）](/previous-versions/sql/sql-server-2008-r2/ms178119(v=sql.105))  
  
- [Kerberos Explained（Kerberos 说明）](/previous-versions/windows/it-pro/windows-2000-server/bb742516(v=technet.10))  
  
<a name="BKMK_q5"></a>

## <a name="when-i-throw-a-faultexceptionexception-where-the-type-is-an-exception-i-always-receive-a-general-faultexception-type-on-the-client-and-not-the-generic-type-whats-happening"></a>当我引发 \<Exception> 类型为异常的 FaultException 时，始终会在客户端上收到常规 FaultException 类型，而不是泛型类型。 发生了什么情况？  

 强烈建议您创建自己的自定义错误数据类型，并在您的错误协定中将其声明为详细信息类型。 原因是使用系统提供的异常类型：  
  
- 创建一个类型依赖，它将移除面向服务的应用程序中功能最强大的应用程序之一。  
  
- 不能依赖以标准方式进行序列化的异常。 某些异常（如 <xref:System.Security.SecurityException>）可能根本无法序列化。  
  
- 向客户端公开内部实现的详细信息。 有关详细信息，请参阅 [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)。  
  
 但是，如果您正在调试应用程序，则可以序列化异常信息，并使用 <xref:System.ServiceModel.Description.ServiceDebugBehavior> 类将其返回到客户端。  
  
<a name="BKMK_q6"></a>

## <a name="it-seems-like-one-way-and-request-reply-operations-return-at-roughly-the-same-speed-when-the-reply-contains-no-data-whats-happening"></a>当答复未包含任何数据时，单向操作与请求-答复操作的返回速度似乎基本相同。 发生了什么情况？  

 指定一个操作为单向只表示操作协定接受输入消息，而不返回输出消息。 在 WCF 中，当出站数据已写入网络或引发异常时，所有客户端调用都将返回。 单向操作以相同方式工作，如果找不到服务则它们可以引发；或者如果服务没有准备好从网络接受数据则它们可以阻止。 通常在 WCF 中，这会导致单向调用返回到客户端的速度比请求-答复更快;但减缓网络上的出站数据发送速度的任何情况都会降低单向操作以及请求-答复操作的速度。 有关详细信息，请参阅单向 [服务](./feature-details/one-way-services.md) 和 [使用 WCF 客户端访问服务](./feature-details/accessing-services-using-a-client.md)。  
  
<a name="BKMK_q77"></a>

## <a name="im-using-an-x509-certificate-with-my-service-and-i-get-a-systemsecuritycryptographycryptographicexception-whats-happening"></a>我对我的服务使用的是 X.509 证书，并且获得一个 System.Security.Cryptography.CryptographicException。 发生了什么情况？  

 这通常发生在更改了 IIS 辅助进程运行时所使用的用户帐户之后。 例如，在 Windows XP 中，如果你将 Aspnet_wp.exe 运行的默认用户帐户更改为自定义用户帐户，你可能会看到此错误。 如果使用的是私钥，则使用该私钥的进程需要具有访问存储该私钥的文件的权限。  
  
 如果的确如此，则必须向该进程的帐户授予对包含该私钥的文件的读访问特权。 例如，如果 IIS 辅助进程运行在 Bob 帐户下，则您需要向 Bob 授予对包含该私钥的文件的读访问权。  
  
 有关如何向正确的用户帐户授予对包含特定 x.509 证书的私钥的文件的访问权限的详细信息，请参阅 [如何：使 X.509 证书可供 WCF 访问](./feature-details/how-to-make-x-509-certificates-accessible-to-wcf.md)。  
  
<a name="BKMK_q88"></a>

## <a name="i-changed-the-first-parameter-of-an-operation-from-uppercase-to-lowercase-now-my-client-throws-an-exception-whats-happening"></a>我将操作的第一个参数从大写更改为了小写，现在我的客户端引发一个异常。 发生了什么情况？  

 操作签名中的参数名称值是协定的一部分，并区分大小写。 当需要区分本地参数名称与描述客户端应用程序操作的元数据时，请使用 <xref:System.ServiceModel.MessageParameterAttribute?displayProperty=nameWithType> 属性。  
  
<a name="BKMK_q99"></a>

## <a name="im-using-one-of-my-tracing-tools-and-i-get-an-endpointnotfoundexception-whats-happening"></a>我正在使用我的跟踪工具之一，并且获得一个 EndpointNotFoundException。 发生了什么情况？  

 如果使用的跟踪工具不是系统提供的 WCF 跟踪机制，并且收到一个 <xref:System.ServiceModel.EndpointNotFoundException> 指示存在地址筛选器不匹配的，则需要使用 <xref:System.ServiceModel.Description.ClientViaBehavior> 类将消息定向到跟踪实用工具，并让该实用工具将这些消息重定向到服务地址。 <xref:System.ServiceModel.Description.ClientViaBehavior> 类更改 `Via` 寻址标头，以独立于最终接收方（由 `To` 寻址标头指示）指定下一个网络地址。 但是，执行此操作时请不要更改终结点地址，它用于设立 `To` 值。  
  
 下面的代码示例演示一个示例客户端配置文件。  
  
```xml
<endpoint
  address="http://localhost:8000/MyServer/"  
  binding="wsHttpBinding"  
  bindingConfiguration="WSHttpBinding_IMyContract"  
  behaviorConfiguration="MyClient"
  contract="IMyContract"
  name="WSHttpBinding_IMyContract">  
</endpoint>  
<behaviors>  
  <endpointBehaviors>  
    <behavior name="MyClient">  
      <clientVia viaUri="http://localhost:8001/MyServer/"/>  
    </behavior>  
  </endpointBehaviors>  
</behaviors>  
```  
  
<a name="BKMK_q10"></a>

## <a name="what-is-the-base-address-how-does-it-relate-to-an-endpoint-address"></a>何为基址？ 它如何关联到一个终结点地址？  

 基址是 <xref:System.ServiceModel.ServiceHost> 类的根地址。 默认情况下，如果将一个 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 类添加到您的服务配置中，主机发布的所有终结点的 Web 服务描述语言 (WSDL) 都将从 HTTP 基址、提供给元数据行为的任何相对地址以及“?wsdl”中检索。 如果你熟悉 ASP.NET 和 IIS，则基址等效于虚拟目录。  
  
## <a name="sharing-a-port-between-a-service-endpoint-and-a-mex-endpoint-using-the-nettcpbinding"></a>使用 NetTcpBinding 共享服务终结点和 MEX 终结点之间的端口  

 如果将服务的基址指定为 net.tcp://MyServer:8080/MyService 并添加以下终结点：  
  
```xml  
<services>  
  <service name="Microsoft.Samples.NetTcp.CalculatorService">  
    <endpoint address="calcsvc" binding ="netTcpBinding" contract="Microsoft.Samples.NetTcp.ICalculator"/>  
    <endpoint address="mex" binding="mexTcpBinding" contract="IMetadataExchange" />  
  </service>  
</services>  
```  
  
 如果按下面的配置代码段所示修改某个 NetTcpBinding 设置：  
  
```xml  
<bindings>  
  <netTcpBinding>  
    <binding closeTimeout="00:01:00" openTimeout="00:01:00" receiveTimeout="00:10:00" sendTimeout="00:01:00" transactionFlow="false" transferMode="Buffered" transactionProtocol="OleTransactions" hostNameComparisonMode="StrongWildcard" listenBacklog="10" maxBufferPoolSize="524288" maxBufferSize="65536" maxConnections="11" maxReceivedMessageSize="65536">  
      <readerQuotas maxDepth="32" maxStringContentLength="8192" maxArrayLength="16384" maxBytesPerRead="4096" maxNameTableCharCount="16384"/>  
      <reliableSession ordered="true" inactivityTimeout="00:10:00" enabled="false"/>  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" protectionLevel="EncryptAndSign"/>  
      </security>  
    </binding>  
  </netTcpBinding>  
</bindings>  
```  
  
 您将看到一条与以下内容类似的错误：未经处理的异常: System.ServiceModel.AddressAlreadyInUseException: IP 终结点 0.0.0.0:9000 上已有侦听器 可使用 MEX 终结点的其他端口指定完全限定的 URL 来解决此错误，如下面的配置代码段中所示：  
  
```xml
<services>  
  <service name="Microsoft.Samples.NetTcp.CalculatorService">  
    <endpoint address="calcsvc" binding ="netTcpBinding" contract="Microsoft.Samples.NetTcp.ICalculator"/>  
    <endpoint address="net.tcp://localhost:9001/servicemodelsamples/mex" binding="mexTcpBinding" contract="IMetadataExchange" />  
  </service>  
</services>  
```  
  
<a name="BK_MK99"></a>

## <a name="when-calling-a-wcf-web-http-application-from-a-wcf-soap-application-the-service-returns-the-following-error-405-method-not-allowed"></a>从 WCF SOAP 应用程序调用 WCF Web HTTP 应用程序时，服务返回以下错误：405 不允许的方法  

  (从 WCF 服务使用和) 的服务调用 WCF Web HTTP 应用程序 <xref:System.ServiceModel.WebHttpBinding> <xref:System.ServiceModel.Description.WebHttpBehavior> 时，可能会生成以下异常： ``Unhandled Exception: System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: The remote server returned an unexpected response: (405) Method Not Allowed.`` 发生此异常的原因是 wcf <xref:System.ServiceModel.OperationContext> 使用传入的覆盖传出的 <xref:System.ServiceModel.OperationContext> 。 若要解决此问题，请 <xref:System.ServiceModel.OperationContextScope> 在 WCF WEB HTTP 服务操作内创建。 例如：  
  
```csharp
public string Echo(string input)  
{  
    using (new OperationContextScope(this.InnerChannel))  
    {  
        return base.Channel.Echo(input);  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [调试 Windows 身份验证错误](./feature-details/debugging-windows-authentication-errors.md)
