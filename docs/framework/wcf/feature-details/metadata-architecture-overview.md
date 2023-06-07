---
title: 元数据体系结构概述
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], overview
ms.assetid: 1d37645e-086d-4d68-a358-f3c5b6e8205e
ms.openlocfilehash: db601a26eb7a7703403432590e359f51231e3ca1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248287"
---
# <a name="metadata-architecture-overview"></a>元数据体系结构概述

Windows Communication Foundation (WCF) 为导出、发布、检索和导入服务元数据提供了丰富的基础结构。 WCF 服务使用元数据来描述如何与服务的终结点交互，以便工具（如 Svcutil.exe）可以自动生成用于访问服务的客户端代码。  
  
 构成 WCF 元数据基础结构的大多数类型都驻留在 <xref:System.ServiceModel.Description> 命名空间中。  
  
 WCF 使用 <xref:System.ServiceModel.Description.ServiceEndpoint> 类来描述服务中的终结点。 您可以使用 WCF 生成服务终结点的元数据，或导入服务元数据以生成 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例。  
  
 WCF 将服务的元数据表示为类型的实例 <xref:System.ServiceModel.Description.MetadataSet> ，该类型的结构与 ws-metadataexchange 中定义的元数据序列化格式密切相关。 <xref:System.ServiceModel.Description.MetadataSet> 类型将实际的服务元数据（如 Web 服务描述语言 (WSDL) 文档、XML 架构文档或 WS-Policy 表达式）作为 <xref:System.ServiceModel.Description.MetadataSection> 实例的集合绑定。 每个 <xref:System.ServiceModel.Description.MetadataSection?displayProperty=nameWithType> 实例都包含特定的元数据方言和标识符。 <xref:System.ServiceModel.Description.MetadataSection?displayProperty=nameWithType> 在其 <xref:System.ServiceModel.Description.MetadataSection.Metadata%2A?displayProperty=nameWithType> 属性中可能包含以下项：  
  
- 原始元数据。  
  
- 一个 <xref:System.ServiceModel.Description.MetadataReference> 实例。  
  
- 一个 <xref:System.ServiceModel.Description.MetadataLocation> 实例。  
  
 <xref:System.ServiceModel.Description.MetadataReference?displayProperty=nameWithType> 实例指向另一个元数据交换 (MEX) 终结点，而 <xref:System.ServiceModel.Description.MetadataLocation?displayProperty=nameWithType> 实例指向使用 HTTP URL 的元数据文档。 WCF 支持使用 WSDL 文档来描述服务终结点、服务协定、绑定、消息交换模式、消息以及服务实现的错误消息。 服务使用的数据类型在 WSDL 文档中使用 XML 架构进行描述。 有关详细信息，请参阅 [架构导入和导出](schema-import-and-export.md)。 您可以使用 WCF 导出和导入服务行为、协定行为和扩展服务功能的绑定元素的 WSDL 扩展。 有关详细信息，请参阅 [导出 WCF 扩展的自定义元数据](../extending/exporting-custom-metadata-for-a-wcf-extension.md)。  
  
## <a name="exporting-service-metadata"></a>导出服务元数据  

 在 WCF 中， *元数据导出* 是描述服务终结点并将其投影到客户端可以用来了解如何使用该服务的并行标准化表示形式的过程。 若要从 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例导出元数据，请使用 <xref:System.ServiceModel.Description.MetadataExporter> 抽象类的实现。 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> 实现生成包装在 <xref:System.ServiceModel.Description.MetadataSet> 实例中的元数据。  
  
 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> 类提供了一个框架，用于生成描述终结点绑定的功能和需求及其关联操作、消息和错误的策略表达式。 在 <xref:System.ServiceModel.Description.PolicyConversionContext> 实例中可捕获这些策略表达式。 然后 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> 实现可以将这些策略表达式附加到它生成的元数据。  
  
 当生成供 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> 实现使用的 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> 对象时，<xref:System.ServiceModel.Description.IPolicyExportExtension> 调入在 <xref:System.ServiceModel.Description.ServiceEndpoint> 的绑定中实现 <xref:System.ServiceModel.Description.PolicyConversionContext> 接口的每个 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType>。 通过在 <xref:System.ServiceModel.Description.IPolicyExportExtension> 类型的自定义实现上实现 <xref:System.ServiceModel.Channels.BindingElement> 接口，可以导出新策略断言。  
  
 <xref:System.ServiceModel.Description.WsdlExporter>类型是 <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> WCF 中包含的抽象类的实现。 <xref:System.ServiceModel.Description.WsdlExporter> 类型使用附加的策略表达式生成 WSDL 元数据。  
  
 若要导出自定义 WSDL 元数据或终结点行为的 WSDL 扩展、协定行为或服务终结点中的绑定元素，可以实现 <xref:System.ServiceModel.Description.IWsdlExportExtension> 接口。 在生成 WSDL 文档时，<xref:System.ServiceModel.Description.WsdlExporter> 为实现 <xref:System.ServiceModel.Description.ServiceEndpoint> 接口的绑定元素、操作行为、协定行为和终结点行为查看 <xref:System.ServiceModel.Description.IWsdlExportExtension> 实例。  
  
## <a name="publishing-service-metadata"></a>发布服务元数据  

 WCF 服务通过公开一个或多个元数据终结点来发布元数据。 使用标准协议（如 MEX 和 HTTP/GET 请求）发布服务元数据可使服务元数据变得可用。 元数据终结点与其他服务终结点类似，都具有地址、绑定和协定。 可以在配置中或在代码中将元数据终结点添加到服务主机。  
  
 若要发布 WCF 服务的元数据终结点，必须首先将服务行为的实例添加 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 到服务。 将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 实例添加到服务，使服务增添了通过公开一个或多个元数据终结点发布元数据的功能。 添加 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 服务行为后，可以公开支持 MEX 协议的元数据终结点或响应 HTTP/GET 请求的元数据终结点。  
  
 若要添加使用 MEX 协议的元数据终结点，请将服务终结点添加到使用名为 IMetadataExchange 的服务协定的服务主机。 WCF 定义 <xref:System.ServiceModel.Description.IMetadataExchange> 具有此服务协定名称的接口。 WS-MetadataExchange 终结点或 MEX 终结点，可以使用类中的静态工厂方法公开的四个默认绑定之一 <xref:System.ServiceModel.Description.MetadataExchangeBindings> ，以匹配 WCF 工具使用的默认绑定，如 Svcutil.exe。 也可以使用自定义绑定配置 MEX 元数据终结点。  
  
 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 使用 <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> 来导出服务中所有服务终结点的元数据。 有关从服务中导出元数据的详细信息，请参阅 [导出和导入元数据](exporting-and-importing-metadata.md)。  
  
 通过将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 实例作为服务主机的扩展添加，<xref:System.ServiceModel.Description.ServiceMetadataExtension> 增强了服务主机。 <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> 提供了元数据发布协议的实现。 还可以使用 <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> 通过访问 <xref:System.ServiceModel.Description.ServiceMetadataExtension.Metadata%2A> 属性来在运行时获取服务的元数据。  
  
> [!CAUTION]
> 如果在应用程序配置文件中添加 MEX 终结点，然后尝试在代码中向服务主机添加 <xref:System.ServiceModel.Description.ServiceMetadataBehavior>，则会得到以下异常：  
>
> System.InvalidOperationException: 在服务 Service1 实现的协定列表中找不到协定名称“ImetadataExchange”。 将 ServiceMetadataBehavior 添加到配置文件或直接添加到 ServiceHost，以启用对该协定的支持。  
>
> 通过在配置文件中 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 或在代码中同时添加终结点和 <xref:System.ServiceModel.Description.ServiceMetadataBehavior>，可以解决此问题。  
>
> 有关 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 在应用程序配置文件中添加的示例，请参阅 [入门](../samples/getting-started-sample.md)。 有关添加代码的示例 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> ，请参阅 [自承载](../samples/self-host.md) 示例。  

> [!CAUTION]
> 当发布公开两个不同服务协定（两个服务协定包含具有相同名称的操作）的服务的元数据时，会引发异常。 例如，如果有一个服务公开了一个名为 ICarService 的服务协定，该服务协定具有一个 Get(Car c) 操作，且同一服务还公开了一个名为 IBookService 的服务协定，该服务协定具有一个 Get(Book b) 操作，则当生成该服务的元数据时，会引发异常或显示错误消息。 若要解决此问题，请执行下列操作之一：  
>
> - 重命名其中的一项操作。
> - 将 <xref:System.ServiceModel.OperationContractAttribute.Name%2A> 设置为其他名称。  
> - 使用 <xref:System.ServiceModel.ServiceContractAttribute.Namespace%2A> 属性将其中一项操作的命名空间设置为其他命名空间。  
  
## <a name="retrieving-service-metadata"></a>检索服务元数据  

 WCF 可以使用标准化协议（如 WS-MetadataExchange 和 HTTP）检索服务元数据。 这两种协议均受 <xref:System.ServiceModel.Description.MetadataExchangeClient> 类型支持。 通过提供地址和可选绑定，使用 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 类型检索服务元数据。 由 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 实例使用的绑定可以是 <xref:System.ServiceModel.Description.MetadataExchangeBindings> 静态类中的默认绑定之一、用户提供的绑定或从 `IMetadataExchange` 协定的终结点配置加载的绑定。 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 也可以使用 <xref:System.Net.HttpWebRequest> 类型来解析 HTTP URL 对元数据的引用。  
  
 默认情况下，<xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 实例与单个 <xref:System.ServiceModel.Channels.ChannelFactoryBase> 实例关联。 通过重写 <xref:System.ServiceModel.Channels.ChannelFactoryBase> 虚拟方法，您可以更改或替换由 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 使用的 <xref:System.ServiceModel.Description.MetadataExchangeClient.GetChannelFactory%2A> 实例。 同样，通过重写 <xref:System.Net.HttpWebRequest?displayProperty=nameWithType> 虚拟方法，可以更改或替换由 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 使用的 <xref:System.ServiceModel.Description.MetadataExchangeClient.GetWebRequest%2A?displayProperty=nameWithType> 实例以发出 HTTP/GET 请求。  
  
 您可以使用 WS-MetadataExchange 或 HTTP/GET 请求检索服务元数据，方法是使用 Svcutil.exe 工具并传递 **/target： metadata** 开关和地址。 Svcutil.exe 下载指定位置的元数据并将文件保存到磁盘。 Svcutil.exe 在内部使用 <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> 实例，并加载其名称与传递到 Svcutil.exe 的地址的方案（如果存在）匹配的 MEX 终结点配置（从应用程序配置文件）。 否则，Svcutil.exe 默认使用由 <xref:System.ServiceModel.Description.MetadataExchangeBindings> 静态工厂类型定义的绑定之一。  
  
## <a name="importing-service-metadata"></a>导入服务元数据  

 在 WCF 中，元数据导入是从其元数据生成服务或其组件部分的抽象表示形式的过程。 例如，WCF 可以 <xref:System.ServiceModel.Description.ServiceEndpoint> 从服务的 WSDL 文档导入实例、实例 <xref:System.ServiceModel.Channels.Binding> 或 <xref:System.ServiceModel.Description.ContractDescription> 实例。 若要在 WCF 中导入服务元数据，请使用 <xref:System.ServiceModel.Description.MetadataImporter> 抽象类的实现。 派生自类的类型 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> 实现了对导入利用 WCF 中的 WS-Policy 导入逻辑的元数据格式的支持。  
  
 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> 实现收集附加到 <xref:System.ServiceModel.Description.PolicyConversionContext> 对象中服务元数据的策略表达式。 然后，通过在 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> 属性中调用 <xref:System.ServiceModel.Description.IPolicyImportExtension> 接口的实现，<xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> 在导入元数据的过程中处理策略。  
  
 通过将自己的 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> 接口实现添加到 <xref:System.ServiceModel.Description.IPolicyImportExtension> 实例上的 <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> 集合，可以添加对将新策略断言导入到 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> 的支持。 或者，可以在客户端应用程序配置文件中注册策略导入扩展。  
  
 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType>类型是 <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> WCF 中包含的抽象类的实现。 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 类型可以导入含有附加策略（这些策略捆绑在 <xref:System.ServiceModel.Description.MetadataSet> 对象中）的 WSDL 元数据。  
  
 通过实现 <xref:System.ServiceModel.Description.IWsdlImportExtension> 接口，然后将实现添加到 <xref:System.ServiceModel.Description.WsdlImporter.WsdlImportExtensions%2A> 实例上的 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 属性，可以添加对导入 WSDL 扩展的支持。 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 还可以加载在客户端应用程序配置文件中注册的 <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> 接口的实现。  
  
## <a name="dynamic-bindings"></a>动态绑定  

 如果终结点的绑定更改，或者希望为使用相同协定但具有不同绑定的终结点创建一个通道，则可以动态更新用来为服务终结点创建通道的绑定。 可以使用 <xref:System.ServiceModel.Description.MetadataResolver> 静态类在运行时为实现特定协定的服务终结点检索和导入元数据。 然后可以使用导入的 <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> 对象为所需终结点创建客户端或通道工厂。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description>
- [元数据格式](metadata-formats.md)
- [导出和导入元数据](exporting-and-importing-metadata.md)
- [发布元数据](publishing-metadata.md)
- [检索元数据](retrieving-metadata.md)
- [使用元数据](using-metadata.md)
- [元数据的安全注意事项](security-considerations-with-metadata.md)
- [扩展元数据系统](../extending/extending-the-metadata-system.md)
