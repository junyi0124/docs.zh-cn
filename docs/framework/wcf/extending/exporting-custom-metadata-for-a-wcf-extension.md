---
title: 导出 WCF 扩展的自定义元数据
ms.date: 03/30/2017
ms.assetid: 53c93882-f8ba-4192-965b-787b5e3f09c0
ms.openlocfilehash: 1aad43eb59a92df9376577ba0108661a0cb9cd87
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249327"
---
# <a name="exporting-custom-metadata-for-a-wcf-extension"></a>导出 WCF 扩展的自定义元数据

在 Windows Communication Foundation (WCF) 中，元数据导出是描述服务终结点并将其投影到客户端可以用来了解如何使用该服务的并行标准化表示形式的过程。 自定义元数据包含系统提供的元数据导出程序无法导出的 XML 元素。 通常，这包括自定义 WSDL 元素（用于用户定义的行为）、绑定元素和策略断言（与绑定和协定的功能和需求有关）。  
  
 本节介绍如何导出自定义 WSDL 或策略断言，而不关注导出过程本身。 有关如何使用导出和导入元数据的类型（无论元数据是自定义的还是系统构造的）的详细信息，请参阅 [导出和导入元数据](../feature-details/exporting-and-importing-metadata.md)。  
  
## <a name="overview"></a>概述  

 当使用发布元数据时 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> ，将 <xref:System.ServiceModel.Description.ServiceDescription?displayProperty=nameWithType> 检查并使用系统提供的属性和绑定为 WCF 可支持的所有协定和绑定生成 XSD 和 WSDL，包括策略断言。 但是，自定义行为属性或绑定元素需要获得支持才能正确导出。  
  
 本节介绍以下内容：  
  
1. 如何实现和使用 <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> 接口，该接口在发布 WSDL 之前向您公开 WSDL 生成数据。  
  
2. 如何实现和使用 <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> 接口，该接口在导出 WSDL 数据中的策略断言之前向您公开策略数据。  
  
 有关导入自定义 WSDL 和策略断言的详细信息，请参阅 [导入 WCF 扩展的自定义元数据](importing-custom-metadata-for-a-wcf-extension.md)。  
  
## <a name="exporting-custom-wsdl-elements"></a>导出自定义 WSDL 元素  

 在操作行为、协定行为、终结点行为或绑定元素（分别为 <xref:System.ServiceModel.Description.IWsdlExportExtension>、<xref:System.ServiceModel.Description.IOperationBehavior>、<xref:System.ServiceModel.Description.IContractBehavior> 或 <xref:System.ServiceModel.Description.IEndpointBehavior>）上实现 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType>，并将行为或绑定元素插入正试图导出的服务的说明中。  (有关插入行为的详细信息，请参阅) 的 [行为配置和扩展运行时](configuring-and-extending-the-runtime-with-behaviors.md) 。 为每个终结点调用 <xref:System.ServiceModel.Description.IWsdlExportExtension>，然后每个终结点首先导出协定（如果协定尚未导出）。 您可以执行以下任一导出过程，具体取决于您的需要：  
  
- 使用 <xref:System.ServiceModel.Description.WsdlContractConversionContext> 修改 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%2A> 方法中导出的元数据。  
  
- 使用 <xref:System.ServiceModel.Description.WsdlEndpointConversionContext> 修改 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A> 方法导出的终结点的元数据。  
  
 在正在导出的 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%2A> 实例中，所有 <xref:System.ServiceModel.Description.IWsdlExportExtension> 实现上都会调用 <xref:System.ServiceModel.Description.ContractDescription?displayProperty=nameWithType> 方法。  在正在导出的 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A> 实例中，所有 <xref:System.ServiceModel.Description.IWsdlExportExtension> 实现上都会调用 <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> 方法。  
  
 有关详细信息，请参阅 [如何：导出自定义 wsdl](how-to-export-custom-wsdl.md) 和示例 [自定义 wsdl 发布](../samples/custom-wsdl-publication.md)。  
  
## <a name="exporting-custom-policy-assertions"></a>导出自定义策略断言  

 在 <xref:System.ServiceModel.Description.IPolicyExportExtension> 上实现 <xref:System.ServiceModel.Channels.BindingElement> 并将绑定元素添加到绑定，可以将有关绑定支持和协定功能的自定义策略断言写入 WSDL。 在导出绑定中的已实现绑定元素后，即会调用 <xref:System.ServiceModel.Description.IPolicyExportExtension>，并将 <xref:System.ServiceModel.Description.PolicyConversionContext> 传递到 <xref:System.ServiceModel.Description.IPolicyExportExtension.ExportPolicy%2A> 方法。 使用 <xref:System.ServiceModel.Description.PolicyConversionContext> 实例的方法，可以向附加到 WSDL 绑定的消息、操作或终结点主题的策略断言进行添加操作。  
  
 有关详细信息，请参阅 [如何：导出自定义策略断言](how-to-export-custom-policy-assertions.md)。  
  
## <a name="see-also"></a>另请参阅

- [如何：导出自定义 WSDL](how-to-export-custom-wsdl.md)
- [如何：导出自定义策略断言](how-to-export-custom-policy-assertions.md)
- [导入 WCF 扩展的自定义元数据](importing-custom-metadata-for-a-wcf-extension.md)
