---
title: WCF 简化功能
ms.date: 03/30/2017
ms.assetid: 4535a511-6064-4da0-b361-80262a891663
ms.openlocfilehash: 8a818ec0852cfae20ef23fede04b55b08a7449a5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732913"
---
# <a name="wcf-simplification-features"></a>WCF 简化功能

本主题讨论可使编写 WCF 应用程序更简单的新功能。

## <a name="simplified-generated-configuration-files"></a>简化了生成的配置文件

当您在 Visual Studio 中添加服务引用或使用 SvcUtil.exe工具时，将会生成一个客户端配置文件。 在以前版本的 WCF 中，这些配置文件包含每个绑定属性的值，即使该值是默认值。 在 WCF 4.5 中，生成的配置文件仅包含那些设置为非默认值的绑定属性。

下面是由 WCF 3.0 生成的配置文件的示例。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.serviceModel>
        <bindings>
            <basicHttpBinding>
                <binding name="BasicHttpBinding_IService1" closeTimeout="00:01:00"
                    openTimeout="00:01:00" receiveTimeout="00:10:00" sendTimeout="00:01:00"
                    allowCookies="false" bypassProxyOnLocal="false"
                    hostNameComparisonMode="StrongWildcard" maxBufferSize="65536"
                    maxBufferPoolSize="524288" maxReceivedMessageSize="65536"
                    messageEncoding="Text" textEncoding="utf-8" transferMode="Buffered"
                    useDefaultWebProxy="true">
                    <readerQuotas maxDepth="32" maxStringContentLength="8192"
                        maxArrayLength="16384" maxBytesPerRead="4096"
                        maxNameTableCharCount="16384" />
                    <security mode="None">
                        <transport clientCredentialType="None" proxyCredentialType="None"
                            realm="" />
                        <message clientCredentialType="UserName" algorithmSuite="Default" />
                    </security>
                </binding>
            </basicHttpBinding>
        </bindings>
        <client>
            <endpoint address="http://localhost:36906/Service1.svc" binding="basicHttpBinding"
                bindingConfiguration="BasicHttpBinding_IService1" contract="IService1"
                name="BasicHttpBinding_IService1" />
        </client>
    </system.serviceModel>
</configuration>
```

下面是由 WCF 4.5 生成的相同配置文件的示例。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.serviceModel>
        <bindings>
            <basicHttpBinding>
                <binding name="BasicHttpBinding_IService1" />
            </basicHttpBinding>
        </bindings>
        <client>
            <endpoint address="http://localhost:36906/Service1.svc" binding="basicHttpBinding"
                bindingConfiguration="BasicHttpBinding_IService1" contract="IService1"
                name="BasicHttpBinding_IService1" />
        </client>
    </system.serviceModel>
</configuration>
```

## <a name="contract-first-development"></a>协定优先开发

WCF 现在支持协定优先开发。 svcutil.exe 工具有一个/serviceContract 开关，可用于从 WSDL 文档生成服务和数据协定。

## <a name="add-service-reference-from-a-portable-subset-project"></a>从可移植子集项目添加服务引用

可移植子集项目使 .NET 程序集编程人员能够维护单个源树和生成系统，同时仍支持 (桌面、Silverlight、Windows Phone 和 Xbox) 的多个 .NET 实现。 可移植子集项目仅引用可以在任何 .NET 实现上使用的程序集的 .NET 可移植库。 开发人员的体验与添加任何其他 WCF 客户端应用程序内的服务引用相同。 有关详细信息，请参阅 [可移植子集项目中的添加服务引用](add-service-reference-in-a-portable-subset-project.md)。

## <a name="aspnet-compatibility-mode-default-changed"></a>ASP.NET 兼容模式默认值已更改

WCF 提供了 ASP.NET 兼容模式，以向开发人员授予编写 WCF 服务时对 ASP.NET HTTP 管道中的功能的完全访问权限。 若要使用此模式，你必须 `aspNetCompatibilityEnabled` 在 web.config 的节中将属性设置为 true [\<serviceHostingEnvironment>](../configure-apps/file-schema/wcf/servicehostingenvironment.md) 。此外，此 appDomain 中的任何服务都需要 `RequirementsMode` 将其上的 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 属性设置为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> 或 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required> 。 默认情况下， <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 将设置为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> ，默认的 WCF 服务应用程序模板会将 `aspNetCompatibilityEnabled` 特性设置为 `true` 。 有关详细信息，请参阅 Windows Communication Foundation 4.5 和[WCF 服务和 ASP.NET](./feature-details/wcf-services-and-aspnet.md)[的新增功能](whats-new.md)。

## <a name="streaming-improvements"></a>流改进

- 向 WCF 添加了对异步流的新支持。 若要启用异步流，请将 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> 终结点行为添加到服务主机，并将其 <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> 属性设置为 `true`。 在服务将经过流处理的消息发送到多个正在缓慢读取的客户端时，这样做有益于可伸缩性。 WCF 不再阻止每个客户端的一个线程，并将释放该线程以便为另一个客户端提供服务。

- 消除了服务由 IIS 承载时缓冲消息方面的限制。 在以前版本的 WCF 中，在收到关于使用流消息传输的 IIS 承载服务的消息时，ASP.NET 会在将该消息发送到 WCF 之前缓冲整个消息。 这将导致消耗大量内存。 此缓冲已在 .NET Framework 4.5 中被删除，现在 IIS 承载的 WCF 服务可以在收到整个消息之前开始处理传入的流，从而启用真正的流式处理。 这样，WCF 就可立即对消息作出响应，并可以提高性能。 此外，您不必再指定 `maxRequestLength` 的值，即 ASP.NET 对传入请求的大小限制。 如果设置了此属性，则会将其忽略。 有关详细信息， `maxRequestLength` 请参阅[ \<httpRuntime> 配置元素](/previous-versions/dotnet/netframework-1.1/e1f13641(v=vs.71))。 你仍需要配置 maxAllowedContentLength，有关详细信息，请参阅 [IIS 请求限制](/previous-versions/iis/settings-schema/ms689462(v=vs.90))。

## <a name="new-transport-default-values"></a>新传输默认值

下表描述了已更改的设置以及可在何处找到其他信息。

|properties|开|新默认值|更多信息|
|--------------|--------|-----------------|----------------------|
|channelInitializationTimeout|<xref:System.ServiceModel.NetTcpBinding>|30 秒|此属性可确定 TCP 连接使用 .NET 组帧协议对自身进行身份验证所需的时间。 客户端需要发送一些初始数据，然后服务器才有足够的信息来执行身份验证。 此超时被有意设置为小于 ReceiveTimeout（10 分钟），这样，未经身份验证的恶意客户端就无法长时间保持绑到该服务器的连接。 默认值为 30 秒。 有关的详细信息 <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.ChannelInitializationTimeout%2A>|
|listenBacklog|<xref:System.ServiceModel.NetTcpBinding>|16 * 处理器数|此套接字级别的属性描述了要排队的“挂起接受”请求的数目。 如果侦听积压工作 (backlog) 队列已满，则会拒绝新的套接字请求。 有关的详细信息 <xref:System.ServiceModel.NetTcpBinding.ListenBacklog%2A>|
|maxPendingAccepts|ConnectionOrientedTransportBindingElement<br /><br /> SMSvcHost.exe|2 * 传输处理器的数目<br /><br /> 4 \* SMSvcHost.exe 的处理器数|此属性会限制服务器可具有的等待侦听器的通道数目。 当 MaxPendingAccepts 太低时，会有一个较小的时间间隔，在此间隔内，所有等待的通道都已开始服务连接，但没有新通道已开始侦听。 连接可在此间隔内到达，但将会因服务器上没有等待它的内容而失败。 可通过将 <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A> 属性设置为较大的数字来配置此属性。 有关详细信息，请参阅 <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingAccepts%2A> 和 [配置 Net.tcp 端口共享服务](./feature-details/configuring-the-net-tcp-port-sharing-service.md)|
|maxPendingConnections|ConnectionOrientedTransportBindingElement|12 * 处理器数目|此属性控制一个传输已接受但 ServiceModel 调度程序尚未选取的连接的数目。 若要设置此值，请对该绑定使用 `MaxConnections` 或对该绑定元素使用 `maxOutboundConnectionsPerEndpoint`。 有关的详细信息 <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A>|
|receiveTimeout|SMSvcHost.exe|30 秒|此属性为读取 TCP 组帧数据并执行来自基础连接的连接调度指定超时值。 此超时值用于对该时间段施加一个限制，SMSvcHost.exe 服务将在该时段内保持从传入连接读取前导码数据。 有关详细信息，请参阅 [配置 Net.tcp 端口共享服务](./feature-details/configuring-the-net-tcp-port-sharing-service.md)。|

> [!NOTE]
> 只有在安装有 .NET Framework 4.5 的计算机上部署 WCF 服务时，才会使用这些新默认值。 如果在安装有 .NET Framework 4.0 的计算机上部署同一服务，则将使用 .NET Framework 4.0 默认值。 在这种情况下，建议显式配置这些设置。

## <a name="xmldictionaryreaderquotas"></a>XmlDictionaryReaderQuotas

<xref:System.Xml.XmlDictionaryReaderQuotas> 包含用于 XML 字典读取器的可配置配额值，这些配额值会限制创建消息时由编码器使用的内存量。 虽然这些配额是可配置的，但默认值已更改，以减小开发人员需要显式设置这些默认值的可能性。 `MaxReceivedMessageSize` 配额未更改，这样它仍可以限制内存消耗，从而您无需处理 <xref:System.Xml.XmlDictionaryReaderQuotas> 的复杂性。 下表显示了配额及其新默认值，并简要说明了每个配额的用途。

|配额名称|默认值|说明|
|----------------|-------------------|-----------------|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>|Int32.MaxValue|获取并设置允许的最大数组长度。 此配额限制 XML 读取器返回的基元数组（包括字节数组）的最大大小。 此配额不限制 XML 读取器本身的内存消耗，但限制使用该读取器的所有组件的内存消耗。 例如，当 <xref:System.Runtime.Serialization.DataContractSerializer> 使用一个以 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>保护的读取器时，将不会反序列化大于此配额的字节数组。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A>|Int32.MaxValue|获取并设置允许每次读取返回的最大字节数。 此配额限制在当读取元素开始标记及其属性时在一次 Read 操作中读取的字节数目。 （在非流模式情况下，不会根据配额对元素名称本身进行计数）。 具有太多的 XML 特性可能会耗用过长的处理时间，因为必须检查特性名称的唯一性。 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A> 可缓解这一威胁。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A>|节点深度为 128|此配额限制 XML 元素的最大嵌套深度。 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> 与 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A>交互：读取器始终在内存中保留当前元素以及它的所有上级的数据，因此读取器的最大内存消耗与这两个设置的积成比例。 当反序列化深度嵌套的对象图时，会迫使反序列化程序访问整个堆栈并引发一个不可恢复的 <xref:System.StackOverflowException>。 对于 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Xml.Serialization.XmlSerializer>，XML 嵌套和对象嵌套之间存在直接关联。 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> 用于缓解这一威胁。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxNameTableCharCount%2A>|Int32.MaxValue|此配额限制名称表中所允许的最大字符数。 名称表包含在处理 XML 文档时遇到的一些字符串（例如，命名空间和前缀）。 由于这些字符串在内存中进行缓冲，因此，此配额用于防止在预期使用流处理时进行过度缓冲。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>|Int32.MaxValue|此配额限制 XML 读取器返回的最大字符串大小。 此配额不限制 XML 读取器本身的内存消耗，但限制使用该读取器的组件中的内存消耗。 例如，当 <xref:System.Runtime.Serialization.DataContractSerializer> 使用一个以 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>保护的读取器时，将不会反序列化大于此配额的字符串。|

> [!IMPORTANT]
> 有关保护数据的详细信息，请参阅 [数据安全注意事项](./feature-details/security-considerations-for-data.md) 下的 "安全地使用 XML"。

> [!NOTE]
> 只有在安装有 .NET Framework 4.5 的计算机上部署 WCF 服务时，才会使用这些新默认值。 如果在安装有 .NET Framework 4.0 的计算机上部署同一服务，则将使用 .NET Framework 4.0 默认值。 在这种情况下，建议显式配置这些设置。

## <a name="wcf-configuration-validation"></a>WCF 配置验证

作为 Visual Studio 中生成过程的一部分，现在将对 WCF 配置文件进行验证。 如果验证失败，则将在 Visual Studio 中显示验证错误或警告的列表。

## <a name="xml-editor-tooltips"></a>XML 编辑器工具提示

为了帮助新的和现有 WCF 服务开发人员配置服务，Visual Studio XML 编辑器现在为属于服务配置文件的每个配置元素及其属性提供了工具提示。

## <a name="basichttpbinding-improvements"></a>BasicHttpBinding 改进

1. 使单个 WCF 终结点能够响应不同身份验证模式。

2. 使 WCF 服务的安全性设置能够由 IIS 进行控制
