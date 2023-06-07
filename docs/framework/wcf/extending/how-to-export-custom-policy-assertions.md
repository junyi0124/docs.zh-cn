---
title: 如何：导出自定义策略断言
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 99030386-43b0-4f7b-866d-17ea307f5cbd
ms.openlocfilehash: 8bdc9948d6846631fde1d428c113682f40d15ec3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249210"
---
# <a name="how-to-export-custom-policy-assertions"></a>如何：导出自定义策略断言

策略断言说明服务终结点的功能和要求。 服务应用程序可以在服务元数据中使用自定义策略断言，来将终结点、绑定或协定自定义信息传递到客户端应用程序。 您可以使用 Windows Communication Foundation (WCF) 在终结点、操作或消息主题的 WSDL 绑定中附加的策略表达式中导出断言，具体取决于您所通信的功能或要求。  
  
 可以通过以下方式导出自定义策略断言：在 <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> 上实现 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> 接口，然后或者将绑定元素直接插入到服务终结点的绑定，或者在应用程序配置文件中注册绑定元素。 策略导出实现应将自定义策略断言作为 <xref:System.Xml.XmlElement?displayProperty=nameWithType> 实例添加到传入 <xref:System.ServiceModel.Description.PolicyAssertionCollection?displayProperty=nameWithType> 方法的 <xref:System.ServiceModel.Description.PolicyConversionContext?displayProperty=nameWithType> 上的相应 <xref:System.ServiceModel.Description.IPolicyExportExtension.ExportPolicy%2A>。  
  
 另外，您需要检查 <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> 类的 <xref:System.ServiceModel.Description.WsdlExporter> 属性，并根据指定的策略版本导出正确的命名空间中的嵌套策略表达式和策略框架属性。  
  
 若要导入自定义策略断言，请参阅 <xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType> 和 [如何：导入自定义策略断言](how-to-import-custom-policy-assertions.md)。  
  
### <a name="to-export-custom-policy-assertions"></a>导出自定义策略断言  
  
1. 在 <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> 上实现 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> 接口。 下面的代码示例演示了在绑定级别实现自定义策略断言。  
  
     [!code-csharp[CustomPolicySample#14](../../../../samples/snippets/csharp/VS_Snippets_CFX/custompolicysample/cs/policyexporter.cs#14)]
     [!code-vb[CustomPolicySample#14](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/custompolicysample/vb/policyexporter.vb#14)]  
  
2. 以编程方式或者使用应用程序配置文件将绑定元素插入到终结点绑定。 请参见下面的过程。  
  
### <a name="to-insert-a-binding-element-using-an-application-configuration-file"></a>使用应用程序配置文件插入绑定元素  
  
1. 为自定义策略断言绑定元素实现 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement?displayProperty=nameWithType>。  
  
2. 使用元素将绑定元素扩展添加到配置文件 [\<bindingElementExtensions>](../../configure-apps/file-schema/wcf/bindingelementextensions.md) 。  
  
3. 使用 <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> 生成一个自定义绑定。  
  
### <a name="to-insert-a-binding-element-programmatically"></a>以编程方式插入绑定元素  
  
1. 创建一个新的 <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> 并将其添加到 <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType>。  
  
2. 将步骤 1 中的自定义绑定添加 到一个新的终结点，并通过调用 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> 方法将该新服务终结点添加到 <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A>。  
  
3. 打开 <xref:System.ServiceModel.ServiceHost>。 下面的代码示例演示如何创建自定义绑定以及如何以编程方式插入绑定元素。  
  
     [!code-csharp[s_imperative#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_imperative/cs/service.cs#1)]
     [!code-vb[s_imperative#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_imperative/vb/service.vb#1)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.IPolicyImportExtension>
- <xref:System.ServiceModel.Description.IPolicyExportExtension>
- [如何：导人自定义策略断言](how-to-import-custom-policy-assertions.md)
