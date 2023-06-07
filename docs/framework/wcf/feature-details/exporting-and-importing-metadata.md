---
title: 导出和导入元数据
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], exporting and importing
ms.assetid: 614a75bb-e0b0-4c95-b6d8-02cb5e5ddb38
ms.openlocfilehash: 44a684ca7904cc059277d94f26b5c077794d75b9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276641"
---
# <a name="exporting-and-importing-metadata"></a>导出和导入元数据

在 Windows Communication Foundation (WCF) 中，导出元数据是描述服务终结点并将其投影到客户端可用来了解如何使用该服务的并行标准化表示形式的过程。 导入服务元数据是一个从服务元数据生成 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例或部分的过程。  
  
## <a name="exporting-metadata"></a>导出元数据  

 若要从 <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> 实例导出元数据，请使用 <xref:System.ServiceModel.Description.MetadataExporter> 抽象类的实现。 <xref:System.ServiceModel.Description.WsdlExporter>类型是 <xref:System.ServiceModel.Description.MetadataExporter> WCF 中包含的抽象类的实现。  
  
 <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> 类型使用封装在 <xref:System.ServiceModel.Description.MetadataSet> 实例中的附加策略表达式生成 Web 服务描述语言 (WSDL) 元数据。 可以使用 <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> 实例以迭代方式为 <xref:System.ServiceModel.Description.ContractDescription> 对象和 <xref:System.ServiceModel.Description.ServiceEndpoint> 对象导出元数据。 还可以导出 <xref:System.ServiceModel.Description.ServiceEndpoint> 对象的集合，并将其与特定的服务名称相关联。  
  
> [!NOTE]
> 只可使用 `WsdlExporter` 从包含公共运行库 (CLR) 类型信息的 `ContractDescription` 实例中导出元数据，例如使用 `ContractDescription` 方法创建的 `ContractDescription.GetContract` 实例，或作为 `ServiceDescription` 实例 `ServiceHost` 的一部分创建的实例。 对于从服务元数据导入的或构建时没有类型信息的`WsdlExporter` 实例，您不能使用 `ContractDescription` 从此类实例导出元数据。  
  
## <a name="importing-metadata"></a>导入元数据  
  
### <a name="importing-wsdl-documents"></a>导入 WSDL 文档  

 若要在 WCF 中导入服务元数据，请使用 <xref:System.ServiceModel.Description.MetadataImporter> 抽象类的实现。 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType>类型是 <xref:System.ServiceModel.Description.MetadataImporter> WCF 中包含的抽象类的实现。 <xref:System.ServiceModel.Description.WsdlImporter> 类型使用捆绑在 <xref:System.ServiceModel.Description.MetadataSet> 对象中的附加策略来导入 WSDL 元数据。  
  
 <xref:System.ServiceModel.Description.WsdlImporter> 类型可让您控制如何导入元数据。 可以导入所有终结点、所有绑定或所有协定。 可以导入与特定的 WSDL 服务、绑定或端口类型相关联的所有终结点。 还可以导入特定的 WSDL 端口的终结点、特定的 WSDL 绑定的绑定或特定的 WSDL 端口类型的协定。  
  
 <xref:System.ServiceModel.Description.WsdlImporter> 还公开 <xref:System.ServiceModel.Description.MetadataImporter.KnownContracts%2A> 属性，可让您指定一组不需要导入的协定。 <xref:System.ServiceModel.Description.WsdlImporter> 使用 <xref:System.ServiceModel.Description.MetadataImporter.KnownContracts%2A> 属性中的协定，而不是从元数据导入具有相同限定名的协定。  
  
### <a name="importing-policies"></a>导入策略  

 <xref:System.ServiceModel.Description.WsdlImporter> 类型收集附加到消息、操作和终结点策略主题的策略表达式，然后使用 <xref:System.ServiceModel.Description.IPolicyImportExtension> 集合中的 <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> 实现导入策略表达式。  
  
 策略导入逻辑自动处理对同一 WSDL 文档中的策略表达式的策略引用，并使用 `wsu:Id` 或 `xml:id` 属性进行标识。 策略导入逻辑通过将策略表达式的大小限制为 4096 个节点，从而防止应用程序进行循环策略应用，此处的节点是以下元素之一：`wsp:Policy`、`wsp:All`、`wsp:ExactlyOne` 和 `wsp:policyReference`。  
  
 策略导入逻辑还自动标准化策略表达式。 嵌套的策略表达式和 `wsp:Optional` 属性不进行标准化。 完成的标准化处理的数量限制为 4096 步，其中的每一步都会产生一个策略断言（即，`wsp:ExactlyOne` 元素的子元素）。  
  
 <xref:System.ServiceModel.Description.WsdlImporter> 类型最多尝试 32 种附加到不同 WSDL 策略主题的备用策略组合。 如果没有完全导入任何组合，则使用第一个组合来构造部分自定义绑定。  
  
## <a name="error-handling"></a>错误处理  

 <xref:System.ServiceModel.Description.MetadataExporter> 和 <xref:System.ServiceModel.Description.MetadataImporter> 类型都公开 `Errors` 属性，该属性可以包含一个在导出和导入过程（可在实现工具时使用）中分别出现的错误和警告消息的集合。  
  
 <xref:System.ServiceModel.Description.WsdlImporter> 类型通常会为在导入过程中捕获的异常引发一个异常，并且将相应的错误添加到其 `Errors` 属性中。 但是 <xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A>、<xref:System.ServiceModel.Description.WsdlImporter.ImportAllBindings%2A>、<xref:System.ServiceModel.Description.WsdlImporter.ImportAllEndpoints%2A> 和 <xref:System.ServiceModel.Description.WsdlImporter.ImportEndpoints%2A> 方法不会引发这些异常，因此，必须检查 `Errors` 属性以确定在调用这些方法时是否发生了任何问题。  
  
 <xref:System.ServiceModel.Description.WsdlExporter> 类型会再次引发在导出过程中捕获的所有异常。 这些异常不会作为 `Errors` 属性中的错误而捕获。 一旦 <xref:System.ServiceModel.Description.WsdlExporter> 引发异常，则会进入错误状态，并且无法重用。 当某个操作因使用了通配符操作而无法导出时以及在遇到重复的绑定名称时，<xref:System.ServiceModel.Description.WsdlExporter> 确实会将警告添加到其 `Errors` 属性中。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：将元数据导入服务终结点](how-to-import-metadata-into-service-endpoints.md)  
 描述如何将下载的元数据导入到说明对象中。  
  
 [如何：从服务终结点导出元数据](how-to-export-metadata-from-service-endpoints.md)  
 描述如何将说明对象导出到元数据中。  
  
 [ServiceDescription 和 WSDL 引用](servicedescription-and-wsdl-reference.md)  
 描述说明对象和 WSDL 之间的映射。  
  
 [如何：使用 Svcutil.exe 将元数据从已编译的服务代码中导出](how-to-use-svcutil-exe-to-export-metadata-from-compiled-service-code.md)  
 描述如何使用 Svcutil.exe 导出已编译程序集中的服务、协定和数据类型的元数据。  
  
 [数据协定架构参考](data-contract-schema-reference.md)  
 描述 <xref:System.Runtime.Serialization.DataContractSerializer> 用来说明 XML 序列化的公共语言运行库 (CLR) 类型的 XML 架构 (XSD) 的子集。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel.Description.WsdlExporter>  
  
 <xref:System.ServiceModel.Description.WsdlImporter>  
  
## <a name="see-also"></a>请参阅

- [导出 WCF 扩展的自定义元数据](../extending/exporting-custom-metadata-for-a-wcf-extension.md)
- [导入 WCF 扩展的自定义元数据](../extending/importing-custom-metadata-for-a-wcf-extension.md)
