---
title: 控制资源使用并提高性能
ms.date: 03/30/2017
ms.assetid: 9a829669-5f76-4c88-80ec-92d0c62c0660
ms.openlocfilehash: f06dd0b7e66ae783b2f268551f15c5e6e8369b7f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255060"
---
# <a name="controlling-resource-consumption-and-improving-performance"></a>控制资源使用并提高性能

本主题介绍 Windows Communication Foundation (WCF) 体系结构的不同区域中的各种属性，这些属性可用于控制资源消耗并影响性能指标。

## <a name="properties-that-constrain-resource-consumption-in-wcf"></a>WCF 中约束资源使用的属性

 Windows Communication Foundation (WCF) 出于安全或性能目的对某些类型的进程应用约束。 这些约束有两种主要形式，即：配额和控制器。 *配额* 是指在达到或超过系统时触发即时异常的限制。 *限制是不* 会立即导致引发异常的限制。 达到控制器限制时，处理仍将继续进行，但不会超过该控制器值所设置的限制。 这一受限处理可能会在另外某处引发异常，但这取决于具体的应用程序。

 除了配额与控制器之间存在区别之外，序列化级别、传输级别以及应用程序级别中也分别存在一些约束属性。 例如，配额 <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A?displayProperty=nameWithType>（由所有系统提供的传输绑定元素实现）默认情况下设置为 65,536 字节，以阻止恶意客户端通过导致过度的内存使用来对服务发起拒绝服务攻击。 （通常，您可以通过降低这一值来提高性能）。

 序列化配额的一个示例是 <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> 属性，该属性指定序列化程序在一次 <xref:System.Runtime.Serialization.DataContractSerializer.ReadObject%2A> 方法调用中序列化或反序列化的对象的最大数目。 应用程序级控制器的一个示例是 <xref:System.ServiceModel.Dispatcher.ServiceThrottle.MaxConcurrentSessions%2A?displayProperty=nameWithType> 属性，该属性默认情况下将并发会话通道连接的数目限定为不超过 10 个。 （与配额不同，如果达到此控制器值，应用程序会继续处理，但不接受新的会话通道，也就是说，在其中某个会话通道终止之前，新客户端将无法连接。）

 这些控制机制旨在针对某些类型的攻击提供现成的缓解，或者改善内存需求量、启动时间等性能指标。 但是，根据不同的应用程序，这些控制机制可能会妨碍服务应用程序性能或者使应用程序根本无法工作。 例如，旨在传送视频的应用程序可能很容易超过默认的 <xref:System.ServiceModel.Channels.TransportBindingElement.MaxReceivedMessageSize%2A?displayProperty=nameWithType> 属性值。 本主题概述了在所有级别的 WCF 上应用于应用程序的各种控件，并介绍了获取有关某个设置是否阻碍您的应用程序的各种方式的详细信息，并介绍了更正各种问题的方法。 大多数控制器以及某些配额都是在应用程序级别提供的，即使基属性是序列化约束或传输约束，也是如此。 例如，可使用 Service 类的 <xref:System.Runtime.Serialization.DataContractSerializer.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> 属性来设置 <xref:System.ServiceModel.ServiceBehaviorAttribute.MaxItemsInObjectGraph%2A?displayProperty=nameWithType> 属性。

> [!NOTE]
> 如果你有特定的问题，则应首先阅读 [WCF 疑难解答快速入门](wcf-troubleshooting-quickstart.md) ，以了解你的问题 (和) 是否列出了解决方案。

 [用于数据的安全注意事项](./feature-details/security-considerations-for-data.md)中列出了限制序列化进程的属性。 限制与传输相关的资源消耗的属性列在 " [传输配额](./feature-details/transport-quotas.md)" 中。 限制应用程序层的资源使用的属性是 <xref:System.ServiceModel.Dispatcher.ServiceThrottle> 类的成员。

## <a name="detecting-application-and-performance-issues-related-to-quota-settings"></a>检测与配额设置有关的应用程序和性能问题

 为了在各种应用程序类型中启用基本的应用程序功能，同时针对常见安全问题提供基本保护，系统选择了上述值的默认值。 但是，不同的应用程序设计可能会超出一个或多个控制器设置，不过可以通过其他手段保护应用程序的安全，并使应用程序按照设计的方式工作。 在这些情况下，必须识别出超过的是哪些控制器值，是在哪一级别超过的，并确定适当的操作过程来提高应用程序的吞吐量。

 通常，当编写并调试应用程序时，应当在配置文件中或者以编程方式将 <xref:System.ServiceModel.Description.ServiceDebugBehavior.IncludeExceptionDetailInFaults%2A?displayProperty=nameWithType> 属性设置为 `true`。 这会指示 WCF 将服务异常堆栈跟踪返回到客户端应用程序以供查看。 此功能可报告大多数的应用程序级异常，旨在显示导致出现问题的相关配额设置。

 一些异常是在运行时发生的，在应用程序层不可见。它们不是使用此机制返回的，并且不能由自定义 <xref:System.ServiceModel.Dispatcher.IErrorHandler?displayProperty=nameWithType> 实现来处理。 如果您使用的是像 Microsoft Visual Studio 这样的开发环境，则这些异常中的大多数都将自动显示。 但是，某些异常可能会被开发环境设置屏蔽，如 [仅我的代码](/visualstudio/debugger/just-my-code) Visual Studio。

 不管开发环境的功能如何，你都可以使用 WCF 跟踪和消息日志记录功能来调试所有异常并调整应用程序的性能。 有关详细信息，请参阅 [使用跟踪对应用程序进行故障排除](./diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md)。

## <a name="performance-issues-and-xmlserializer"></a>性能问题和 XmlSerializer

 如果服务和客户端应用程序使用可用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化的数据类型，则会在运行时生成并编译这些数据类型的序列化代码，从而导致启动性能降低。

> [!NOTE]
> 预生成的序列化代码只能在客户端应用程序中使用，不能在服务中使用。

 通过从应用程序的编译的程序集生成必要的序列化代码， [ ( # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md) 可提高这些应用程序的启动性能。 有关详细信息，请参阅 [如何：使用 XmlSerializer 改善 WCF 客户端应用程序的启动时间](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)。

## <a name="performance-issues-when-hosting-wcf-services-under-aspnet"></a>在 ASP.NET 下承载 WCF 服务时的性能问题

当在 IIS 和 ASP.NET 下承载 WCF 服务时，IIS 和 ASP.NET 的配置设置可能会影响 WCF 服务的吞吐量和内存需求量。  有关 ASP.NET 性能的详细信息，请参阅 [改善 ASP.NET 性能](/previous-versions/msp-n-p/ff647787(v=pandp.10))。 可能具有意外结果的一种设置为 <xref:System.Web.Configuration.ProcessModelSection.MinWorkerThreads%2A>，该设置为 <xref:System.Web.Configuration.ProcessModelSection> 的属性。 如果应用程序具有固定量或少量的客户端，则将 <xref:System.Web.Configuration.ProcessModelSection.MinWorkerThreads%2A> 设置为 2 可能会显著提高 CPU 利用率接近 100% 的多处理器计算机上的吞吐量。 这种性能上的提高是以增加内存使用为代价的，这可能会降低可伸缩性。

## <a name="see-also"></a>另请参阅

- [管理和诊断](./diagnostics/index.md)
- [大型数据和流](./feature-details/large-data-and-streaming.md)
