---
title: Windows Communication Foundation 4.5 中的新增功能
description: 本文介绍 Windows Communication Foundation (WCF) 版本4.5 的新增功能，并提供指向其他资源的链接。
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], what's new
- Windows Communication Foundation [WCF], what's new
ms.assetid: 7e93fe73-af93-46b5-9f63-32f761ee40cf
ms.openlocfilehash: 76b52cedd1e2e64805e2ad47e582d07ca70415cc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731548"
---
# <a name="whats-new-in-windows-communication-foundation-45"></a>Windows Communication Foundation 4.5 中的新增功能

本主题讨论 Windows Communication Foundation (WCF) 版本4.5 的新增功能。

## <a name="wcf-simplification-features"></a>WCF 简化功能

人们已进行了大量工作，以便更加方便地开发和维护 WCF 4.5 应用程序。 有关详细信息，请参阅 [WCF 简化功能](wcf-simplification-features.md)。

### <a name="task-based-async-support"></a>基于任务的异步支持

默认情况下，“添加服务应用”会生成返回任务的异步服务操作方法。 此操作是针对同步和异步方法执行的。 通过此操作，你可以使用新的基于任务的异步编程模型来异步调用服务操作。 在你调用生成的代理方法时，WCF 会构造一个 Task 对象以表示异步操作并返回该任务。 该任务在操作完成时完成。 实现异步操作时，可以将其实现为基于任务的异步操作。 有关详细信息，请参阅 [同步和异步操作](synchronous-and-asynchronous-operations.md)。

### <a name="simplified-generated-configuration-files"></a>简化了生成的配置文件

当您在 Visual Studio 中添加服务引用或使用 SvcUtil.exe工具时，将会生成一个客户端配置文件。 在以前版本的 WCF 中，这些配置文件包含每个绑定属性的值，即使该值是默认值。 在 WCF 4.5 中，生成的配置文件仅包含那些设置为非默认值的绑定属性。

有关详细信息，请参阅 [WCF 简化功能](wcf-simplification-features.md)。

### <a name="contract-first-development"></a>协定优先开发

WCF 现在支持协定优先开发。 svcutil.exe 提供了/serviceContract 开关，使你可以从 WSDL 文档生成服务和数据协定。

### <a name="add-service-reference-from-a-portable-subset-project"></a>从可移植子集项目添加服务引用

可移植子集项目使 .NET 程序集编程人员能够维护单个源树和生成系统，同时仍支持 (桌面、Silverlight、Windows Phone 和 Xbox) 的多个 .NET 平台。 可移植子集项目仅引用可以在任何 .NET 平台上使用的程序集的 .NET 可移植库。 开发人员的体验与添加任何其他 WCF 客户端应用程序内的服务引用相同。 有关详细信息，请参阅 [可移植子集项目中的添加服务引用](add-service-reference-in-a-portable-subset-project.md)。

### <a name="aspnet-compatibility-mode-default-changed"></a>ASP.NET 兼容模式默认值已更改

WCF 提供了 ASP.NET 兼容模式，以向开发人员授予编写 WCF 服务时对 ASP.NET HTTP 管道中的功能的完全访问权限。 若要使用此模式，你必须 `aspNetCompatibilityEnabled` 在 web.config 的节中将属性设置为 true [\<serviceHostingEnvironment>](../configure-apps/file-schema/wcf/servicehostingenvironment.md) 。此外，此 appDomain 中的任何服务都需要 `RequirementsMode` 将其上的 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 属性设置为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> 或 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required> 。 默认情况下， <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> 设置为 <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> 。 有关详细信息，请参阅 [WCF 服务和 ASP.NET](./feature-details/wcf-services-and-aspnet.md)。

### <a name="new-transport-default-values"></a>新传输默认值

为了简化配置，更改了多个传输属性默认值。 有关详细信息，请参阅 [WCF 简化功能](wcf-simplification-features.md)。

### <a name="xmldictionaryreaderquotas"></a>XmlDictionaryReaderQuotas

<xref:System.Xml.XmlDictionaryReaderQuotas> 包含用于 XML 字典读取器的可配置配额值，这些配额值会限制创建消息时由编码器使用的内存量。 虽然这些配额是可配置的，但默认值已更改，以减小开发人员必须显式设置这些默认值的可能性。 有关详细信息，请参阅 [WCF 简化功能](wcf-simplification-features.md)。

### <a name="wcf-configuration-validation"></a>WCF 配置验证

作为 Visual Studio 中生成过程的一部分，现在将针对项目中定义的特性来验证 WCF 配置文件。 如果验证失败，则将在 Visual Studio 中显示验证错误或警告的列表。

### <a name="xml-editor-tooltips"></a>XML 编辑器工具提示

为了帮助新的和现有 WCF 服务开发人员配置服务，Visual Studio XML 编辑器现在为属于服务配置文件的每个配置元素及其属性提供了工具提示。

## <a name="streaming-improvements"></a>流改进

添加了对真实异步流的支持，在这种流中，如果接收方没有读取或者读取速度较慢，则发送方现在不会阻止线程，从而提高了可伸缩性。 消除了客户端向 IIS 承载的 WCF 服务发送流消息时对消息缓冲的限制。 有关详细信息，请参阅 [WCF 简化功能](wcf-simplification-features.md)。

## <a name="simplifying-exposing-an-endpoint-over-https-with-iis"></a>简化通过 HTTPS 与 IIS 公开终结点

为了简化通过 HTTPS 来公开终结点，添加了一个 HTTPS 协议映射。 若要启用 HTTPS 终结点，请确保你的网站已配置 HTTPS 绑定和 SSL 证书，然后，只需为承载该服务的虚拟目录启用 HTTPS。 如果为服务启用了元数据，则也会通过 HTTPS 公开该元数据。

## <a name="generating-a-single-wsdl-document"></a>生成单个 WSDL 文档

某些第三方 WSDL 处理堆栈不能通过 xsd:import 来处理对其他文档有依赖关系的 WSDL 文档。 您现在可通过 WCF 来指定在单个文档中返回的所有 WSDL 信息。 若要请求单个 WSDL 文档，请在从服务请求元数据时将 "？ singleWSDL" 追加到 URI。

## <a name="websocket-support"></a>WebSocket 支持

WebSocket 是一种通过端口 80 和 443 提供真正双向通信的技术，其性能特性与 TCP 类似。 添加了两个新的绑定以支持通过 WebSocket 传输进行通信。 <xref:System.ServiceModel.NetHttpBinding> 和 <xref:System.ServiceModel.NetHttpsBinding>。 有关详细信息，请参阅： [系统提供的绑定](system-provided-bindings.md)。

## <a name="new-transport-default-values"></a>新传输默认值

下表描述了已更改的设置以及可在何处找到其他信息。

|properties|开|新默认值|有关详细信息，请参阅|
|--------------|--------|-----------------|------------------------------|
|channelInitializationTimeout|<xref:System.ServiceModel.NetTcpBinding>|30 秒|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.ChannelInitializationTimeout%2A>|
|listenBacklog|<xref:System.ServiceModel.NetTcpBinding>|12 * 处理器数目|<xref:System.ServiceModel.NetTcpBinding.ListenBacklog%2A>|
|maxPendingAccepts|ConnectionOrientedTransportBindingElement<br /><br /> SMSvcHost.exe|2 * 传输处理器的数目<br /><br /> 4 \* SMSvcHost.exe 的处理器数|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingAccepts%2A> [配置 Net.TCP 端口共享服务](./feature-details/configuring-the-net-tcp-port-sharing-service.md)|
|maxPendingConnections|ConnectionOrientedTransportBindingElement|12 * 处理器数目|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A>|
|receiveTimeout|SMSvcHost.exe|30 秒|[配置 Net.TCP 端口共享服务](./feature-details/configuring-the-net-tcp-port-sharing-service.md)|

## <a name="xml-editor-tooltips"></a>XML 编辑器工具提示

为了帮助新的和现有 WCF 服务开发人员配置服务，Visual Studio XML 编辑器现在为属于服务配置文件的每个配置元素及其属性提供了工具提示。

## <a name="configuring-wcf-services-in-code"></a>在代码中配置 WCF 服务

Windows Communication Foundation (WCF) 允许开发人员使用配置文件或代码来配置服务。 当部署之后需要对服务进行配置时，配置文件十分有用。 在使用配置文件时，IT 专业人员只需要更新配置文件，无需重新编译。 不过，配置文件可能十分复杂，难以维护。 不支持对配置文件进行调试，并且将按名称来引用配置元素，这使得配置文件的创作易于出错且较为困难。 WCF 还允许您在代码中配置服务。 在早期版本的 WCF (4.0 及更早版本中) 在代码中配置服务是很容易的自承载方案， <xref:System.ServiceModel.ServiceHost> 类允许你在调用 ServiceHost 之前配置终结点和行为。 但是，在 Web 承载方案中，您没有访问 <xref:System.ServiceModel.ServiceHost> 类的权限。 若要配置 Web 承载的服务，你需要创建 `System.ServiceModel.ServiceHostFactory`，后者会创建 <xref:System.ServiceModel.Activation.ServiceHostFactory> 并执行任何所需的配置。 从 .NET Framework 4.5 开始，WCF 提供了一种更简单的方法来在代码中配置自承载服务和 web 托管服务。 有关详细信息，请参阅 [在代码中配置 WCF 服务](configuring-wcf-services-in-code.md)。

## <a name="channelfactory-caching"></a>ChannelFactory 缓存

WCF 客户端应用程序使用 <xref:System.ServiceModel.ChannelFactory%601> 类来创建 WCF 服务的通信通道。 创建 <xref:System.ServiceModel.ChannelFactory%601> 实例会带来一定的开销，因为这涉及以下操作：

1. 构造 <xref:System.ServiceModel.Description.ContractDescription> 树

2. 反映所有所需的 CLR 类型

3. 构造通道堆栈

4. 资源的释放

为了尽量减少这种开销，WCF 可以缓存使用 WCF 客户端代理时的通道工厂。 有关详细信息，请参阅 [通道工厂和缓存](./feature-details/channel-factory-and-caching.md)。

## <a name="compression-and-the-binary-encoder"></a>压缩和二进制编码器

从 WCF 4.5 开始，WCF 二进制编码器添加了对压缩的支持。 可使用 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A> 属性来配置压缩类型。 客户端和服务都必须配置 <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A> 属性。 压缩对 HTTP、HTTPS 和 TCP 协议有效。 如果客户端指定要使用压缩，但服务不支持，则会引发协议异常，指示协议不匹配。 有关详细信息，请参阅 [选择消息编码器](./feature-details/choosing-a-message-encoder.md)。

## <a name="udp"></a>UDP

添加了对 UDP 传输的支持，使开发人员能够编写使用 "火灾和遗忘" 消息传送的服务。 客户端向服务发送消息，且不希望从该服务获得响应。

## <a name="multiple-authentication-support"></a>多个身份验证支持

添加了在使用 HTTP 传输和传输安全性时对单个 WCF 终结点上多个 IIS 支持的身份验证模式的支持。 通过 IIS，您可以在虚拟目录上启用多个身份验证模式，此功能可使单个 WCF 终结点支持为承载该 WCF 服务的虚拟目录启用的多个身份验证模式。

## <a name="idn-support"></a>IDN 支持

添加了允许使用带有国际化域名的 WCF 服务的支持。 有关详细信息，请参阅 [WCF 和国际化域名](./feature-details/wcf-and-internationalized-domain-names.md)。

## <a name="httpclient"></a>HttpClient

添加了一个名为 <xref:System.Net.Http.HttpClient> 的新类，以便更加方便地与 HTTP 请求结合使用。 有关详细信息，请参阅 [使应用社交并连接到 http 服务](https://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-581T) 和 [Http 客户端示例](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664)。

## <a name="configuration-intellisense"></a>配置 Intellisense

项目中定义的自定义特定的配置文件中的特性值现在支持 Intellisense，以便快速、准确地使用配置。

## <a name="configuration-tooltips"></a>配置工具提示

WCF 元素和属性现在在 "XML 编辑器" 中具有工具提示，以便更轻松、更准确地标识元素或属性的用途。

## <a name="paste-data-as-classes"></a>将数据作为类进行粘贴

在 WCF 项目中，在 XML 中定义的数据类型（在服务中公开的类型）可以粘贴直接到代码页中。 XML 类型将作为 CLR 类型进行粘贴。 有关更多详细信息，请参阅 [从 XML 生成数据类型类](generating-data-type-classes-from-xml.md) 。

## <a name="webservicehost-and-default-endpoints"></a>WebServiceHost 和默认终结点

在 Visual Studio 2010 中，WebServiceHost 会自动创建默认终结点，而无论你是否显式指定了终结点。 在 Visual Studio 2012 及更高版本中，如果未显式添加任何终结点，则 WebServiceHost 仅创建默认终结点。 如果客户端预期为默认终结点，则可以显式添加终结点，并将客户端指向该终结点。 或者，您可以通过向应用程序的配置文件添加以下设置来告诉 WCF 恢复到上一个行为。

```xml
<appSettings>
    <add key="wcf:webservicehost:enableautomaticendpointscompatability" value="true"/>
  </appSettings>
```

## <a name="ihttpcookiecontainermanager"></a>IHttpCookieContainerManager

由 <xref:System.ServiceModel.Channels.IChannelFactory%601> 公开的此接口可使在客户端上使用 Cookie 变得十分容易。 在针对该绑定将 AllowCookies 设置为 true 之后，你可使用以下代码来访问 Cookie：

```csharp
IHttpCookieContainerManager cookieManager = factory.GetProperty<IHttpCookieContainerManager>();
System.Net.CookieContainer container = cookieManager.CookieContainer;
```

然后，您可以从 <xref:System.Net.CookieContainer> 来检索或设置 Cookie。 若将 AllowCookies 设置为 false，则您可以使用 <xref:System.ServiceModel.OperationContext> 来检索 Cookie，并使用其他 <xref:System.ServiceModel.OperationContext> 或消息检查器，在其他请求中将其发送。 通过 IHttpCookieContainerManager 接口，您可以在一个服务中对用户进行身份验证，并使用由该服务返回的身份验证 Cookie，在其他服务中进行身份验证。
