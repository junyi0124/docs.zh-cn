---
title: ServiceDescription 和 WSDL 引用
ms.date: 03/30/2017
ms.assetid: eedc025d-abd9-46b1-bf3b-61d2d5c95fd6
ms.openlocfilehash: 11a5d65026d13db56d1d349c130861094c3e123b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253877"
---
# <a name="servicedescription-and-wsdl-reference"></a>ServiceDescription 和 WSDL 引用

本主题介绍 Windows Communication Foundation (WCF) 如何将 Web Services 描述语言 (WSDL) 文档映射到 <xref:System.ServiceModel.Description.ServiceDescription> 实例。  
  
## <a name="how-servicedescription-maps-to-wsdl-11"></a>ServiceDescription 如何映射到 WSDL 1.1  

 可以使用 WCF 从服务的实例中导出 WSDL 文档 <xref:System.ServiceModel.Description.ServiceDescription> 。 发布元数据终结点时，将为服务自动生成 WSDL 文档。  
  
 还可以使用 <xref:System.ServiceModel.Description.ServiceEndpoint> 类型从 WSDL 文档导入 <xref:System.ServiceModel.Description.ContractDescription> 实例、<xref:System.ServiceModel.Channels.Binding> 实例和 `WsdlImporter` 实例。  
  
 WCF 导出的 WSDL 文档导入外部 XML 架构文档中使用的所有 XML 架构定义。 为服务中数据类型使用的每个目标命名空间导出单独的 XML 架构文档。 同样，为服务协定使用的每个目标命名空间导出单独的 WSDL 文档。  
  
### <a name="servicedescription"></a>ServiceDescription  

 <xref:System.ServiceModel.Description.ServiceDescription> 实例映射到 `wsdl:service` 元素。 <xref:System.ServiceModel.Description.ServiceDescription> 实例包含 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例的集合，其中每个实例都映射到单独的 `wsdl:port` 元素。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsdl:service` /@name 服务的值。|  
|`Namespace`|服务的 `wsdl:service` 定义的 targetNamespace。|  
|`Endpoints`|服务的 `wsdl:port` 定义。|  
  
### <a name="serviceendpoint"></a>ServiceEndpoint  

 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例映射到 `wsdl:port` 元素。 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例包含一个地址、绑定和协定。  
  
 实现 <xref:System.ServiceModel.Description.IWsdlExportExtension> 接口的终结点行为可以修改它们所附加到的终结点的 `wsdl:port` 元素。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsdl:port` /@name 终结点的值和 `wsdl:binding` /@name 终结点绑定的值。|  
|`Address`|终结点的 `wsdl:port` 定义的地址。<br /><br /> 终结点的传输确定地址的格式。 例如，对于支持 WCF 的传输，可以是 SOAP 地址或终结点引用。|  
|`Binding`|终结点的 `wsdl:binding` 定义。<br /><br /> 与 `wsdl:binding` 定义不同，WCF 中的绑定不与任何一个协定关联。|  
|`Contract`|终结点的 `wsdl:portType` 定义。|  
|`Behaviors`|实现 <xref:System.ServiceModel.Description.IWsdlExportExtension> 接口的终结点行为可以修改终结点的 `wsdl:port`。|  
  
### <a name="bindings"></a>绑定  

 `ServiceEndpoint` 实例的绑定实例映射到 `wsdl:binding` 定义。 与 `wsdl:binding` 定义（必须与特定定义关联）不同 `wsdl:portType` ，WCF 绑定独立于任何协定。  
  
 绑定由绑定元素的集合组成。 每个元素描述终结点与客户端的通信方式的某一方面。 另外，绑定还具有一个 <xref:System.ServiceModel.Channels.MessageVersion>，指示终结点的 <xref:System.ServiceModel.EnvelopeVersion> 和 <xref:System.ServiceModel.Channels.AddressingVersion>。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|在终结点的默认名称中使用，该名称是以下划线分隔追加的协定名称的绑定名称。|  
|`Namespace`|`targetNamespace` 定义的 `wsdl:binding`。<br /><br /> 导入时，如果将策略附加到 WSDL 端口，则导入的绑定命名空间将映射到 `targetNamespace` 定义的 `wsdl:port`。|  
|`BindingElementCollection`，由 `CreateBindingElements`() 方法返回 |`wsdl:binding` 定义的各种域特定的扩展，通常是策略断言。|  
|`MessageVersion`|终结点的 `EnvelopeVersion` 和 `AddressingVersion`。<br /><br /> 如果指定 `MessageVersion.None`，则 WSDL 绑定不包含 SOAP 绑定，并且 WSDL 端口不包含 WS-Addressing 内容。 该设置通常用于 Plain Old XML (POX) 终结点。|  
  
#### <a name="bindingelements"></a>BindingElements  

 终结点绑定的绑定元素映射到 `wsdl:binding` 中的各种 WSDL 扩展，如策略断言。  
  
 绑定的 <xref:System.ServiceModel.Channels.TransportBindingElement> 为 SOAP 绑定确定传输统一资源标识符 (URI)。  
  
#### <a name="addressingversion"></a>AddressingVersion  

 绑定上的 `AddressingVersion` 映射到 `wsd:port` 中使用的寻址版本。 WCF 支持 SOAP 1.1 和 SOAP 1.2 地址，并 WS-Addressing 08/2004 和 WS-Addressing 1.0 终结点引用。  
  
#### <a name="envelopeversion"></a>EnvelopeVersion  

 绑定上的 `EnvelopeVersion` 映射到 `wsdl:binding` 中使用的 SOAP 的版本。 WCF 支持 SOAP 1.1 和 SOAP 1.2 绑定。  
  
### <a name="contracts"></a>协定  

 <xref:System.ServiceModel.Description.ContractDescription> 实例的 `ServiceEndpoint` 实例映射到 `wsdl:portType`。 `ContractDescription` 实例描述给定协定的所有操作。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsdl:portType` /@name 协定的值。|  
|`Namespace`|`wsdl:portType` 定义的 targetNamespace。|  
|`SessionMode`|`wsdl:portType` /@msc:usingSession 协定的值。 此属性是 WSDL 1.1 的 WCF 扩展。|  
|`Operations`|协定的 `wsdl:operation` 定义。|  
  
### <a name="operations"></a>操作  

 <xref:System.ServiceModel.Description.OperationDescription>实例映射到 `wsdl:portType` / `wsdl:operation` 。 `OperationDescription` 包含用于描述操作消息的 `MessageDescription` 实例的集合。  
  
 如下两个操作行为广泛地参与 `OperationDescription` 到 WSDL 文档的映射方式：`DataContractSerializerOperationBehavior` 和 `XmlSerializerOperationBehavior`。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsdl:portType` / `wsdl:operation` /@name 操作的值。|  
|`ProtectionLevel`|附加到此操作的 `wsdl:binding/wsdl:operation` 消息的安全策略中的保护断言。|  
|`IsInitiating`|`wsdl:portType` / `wsdl:operation` /@msc:isInitiating 操作的值。 此属性是 WSDL 1.1 的 WCF 扩展。|  
|`IsTerminating`|`wsdl:portType` / `wsdl:operation` /@msc:isTerminating 操作的值。 此属性是 WSDL 1.1 的 WCF 扩展。|  
|`Messages`|`wsdl:portType` / `wsdl:operation` / `wsdl:input` 操作的和 `wsdl:portType` / `wsdl:operation` / `wsdl:output` 消息。|  
|`Faults`|`wsdl:portType` / `wsdl:operation` / `wsdl:fault` 操作的定义。|  
|`Behaviors`|`DataContractSerializerOperationBehavior` 和 `XmlSerializerOperationBehavior` 处理操作绑定和操作消息。|  
  
#### <a name="the-datacontractserializeroperationbehavior"></a>DataContractSerializerOperationBehavior  

 操作的 `DataContractSerializerOperationBehavior` 是用于为该操作导出 WSDL 消息和绑定的 `IWsdlExportExtension` 实现。 XML 架构类型是使用 `XsdDataContractExporter` 导出的。 `DataContractSerializerOperationBehavior` 还为该操作确定用途、样式和要使用的架构导出程序与导入程序。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`DataContractFormatAttribute`|`Style`此属性的属性将映射到 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@style 操作的值。<br /><br /> `DataContractSerializerOperationBehavior` 仅支持 WSDL 中架构类型的文字用法。|  
  
#### <a name="the-xmlserializeroperationbehavior"></a>XmlSerializerOperationBehavior  

 操作的 `XmlSerializerOperationBehavior` 是用于为该操作导出 WSDL 消息和绑定的 `IWsdlExportExtension` 实现。 XML 架构类型是使用 `XmlSchemaExporter` 导出的。 `XmlSerializerOperationBehavior` 还为该操作确定用途、样式和要使用的架构导出程序与导入程序。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`XmlSerializerFormatAttribute`|`Style`此属性的属性将映射到 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@style 操作的值。<br /><br /> `Use`此属性的属性将映射到 `wsdl:binding` / `wsdl:operation` / `soap:operation` /@use 操作中所有消息的/* 值。|  
  
### <a name="messages"></a>消息  

 `MessageDescription`实例映射到 `wsdl:message` `wsdl:portType` / `wsdl:operation` / `wsdl:input` 或 `wsdl:portType` / `wsdl:operation` / `wsdl:output` 操作中的消息所引用的。 `MessageDescription` 包含正文和标头。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Action`|消息的 SOAP 或 WS-Addressing 操作。<br /><br /> 请注意，使用操作字符串“*”的操作不用 WSDL 表示。|  
|`Direction`|`MessageDirection.Input` 映射到 `wsdl:input`。<br /><br /> `MessageDirection.Output` 映射到 `wsdl:output`。|  
|`ProtectionLevel`|附加到此消息的 `wsdl:message` 定义的安全策略中的保护断言。|  
|`Body`|消息的消息正文。|  
|`Headers`|消息的标头。|  
|`ContractDescription.Name`, `OperationContract.Name`|导出时，用于派生 `wsdl:message` /@name 值。|  
  
#### <a name="message-body"></a>消息正文  

 `MessageBodyDescription`实例映射到 `wsdl:message` / `wsdl:part` 消息正文的定义。 消息正文可以包装也可以裸露。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`WrapperName`|如果样式不是 RPC，则将 `WrapperName` 映射到的引用的元素名称， `wsdl:message` / `wsdl:part` 并 @name 将设置为 "parameters"。|  
|`WrapperNamespace`|如果样式不是 RPC，则将 `WrapperNamespace` 映射到的元素命名空间， `wsdl:message` / `wsdl:part` 并 @name 将设置为 "parameters"。|  
|`Parts`|此消息正文的消息部分。|  
|`ReturnValue`|包装元素的子元素（如果存在包装元素 (文档包装样式）或 RPC 样式) ，否则为消息中的第一个元素 `wsdl:message` / `wsdl:part` 。|  
  
#### <a name="message-parts"></a>消息部分  

 `MessagePartDescription`实例映射到 `wsdl:message` / `wsdl:part` ，以及消息部分所指向的 XML 架构类型或元素。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsd:message` / `wsdl:part` /@name 消息部分的值以及消息部分所指向的元素的名称。|  
|`Namespace`|消息部分所指向的元素的命名空间。|  
|`Index`|消息的的索引 `wsdl:message` / `wsdl:part` 。|  
|`ProtectionLevel`|附加到此消息部分的 `wsdl:message` 定义的安全策略中的保护断言。 将策略参数化，使其指向特定的消息部分。|  
|`MessageType`|消息部分所指向的元素的 XML 架构类型。|  
  
#### <a name="message-headers"></a>消息头  

 `MessageHeaderDescription` 实例是同时还映射到消息部分的 `soap:header` 绑定的消息部分。  
  
### <a name="faults"></a>错误  

 `FaultDescription`实例映射到 `wsdl:portType` / `wsdl:operation` / `wsdl:fault` 定义及其关联 `wsdl:message` 定义。 将 `wsdl:message` 添加到与其关联的 WSDL 端口类型相同的目标命名空间。 `wsdl:message` 有一个名为“详细信息”的消息部分，该部分指向对应于 `DefaultType` 实例的 `FaultDescription` 属性值的 XML 架构元素。  
  
|属性|WSDL 映射|  
|----------------|------------------|  
|`Name`|`wsdl:portType` / `wsdl:operation` / `wsdl:fault` /@name 错误的值。|  
|`Namespace`|错误详细消息部分所指向的 XML 架构元素的命名空间。|  
|`Action`|错误的 SOAP 或 WS-Addressing 操作。|  
|`ProtectionLevel`|附加到此错误的 `wsdl:message` 定义的安全策略中的保护断言。|  
|`DetailType`|详细消息部分所指向的元素的 XML 架构类型。|  
|`Name, ContractDescription.Name, OperationDescription.Name,`|用于派生 `wsdl:message` /@name 错误消息的值。|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description>
