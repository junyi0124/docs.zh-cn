---
title: ServiceModel 属性和 ServiceDescription 引用
ms.date: 03/30/2017
ms.assetid: 4ab86b17-eab9-4846-a881-0099f9a7cc64
ms.openlocfilehash: db2824d7452f1ec4c675782e18ad25bd4be6a483
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253864"
---
# <a name="servicemodel-attributes-and-servicedescription-reference"></a>ServiceModel 属性和 ServiceDescription 引用

*说明树* 是 (类的层次结构，该层次结构从 <xref:System.ServiceModel.Description.ServiceDescription?displayProperty=nameWithType> 类) ，共同描述了服务的各个方面。 Windows Communication Foundation (WCF) 使用说明树来构建有效的服务运行时，以发布 Web 服务描述语言 (WSDL) 、XML 架构定义语言 (XSD) 和策略断言 (元数据) 有关客户端可用于连接和使用服务的服务的信息，以及生成说明树值的各种代码和配置文件表示形式。  
  
 本主题介绍如何从服务协定获取与协定相关的属性，如何实现这些属性，以及如何将这些属性添加到说明树。 在某些情况下，特性值会转换为行为属性，然后行为会插入到说明树中。 有关如何将说明树值转换为元数据的详细信息，请参阅 [ServiceDescription 和 WSDL 引用](servicedescription-and-wsdl-reference.md)。  
  
## <a name="mapping-operations-to-the-description-tree"></a>将操作映射到说明树  

 在 WCF 应用程序中，服务协定按) 使用属性将接口或类及其方法标记为操作分组的接口 (或类进行建模。 <xref:System.ServiceModel.ServiceHost> 类打开后，所有服务协定和实现都会反映出来，并和配置信息一起合并到说明树中。  
  
 有两种类型的操作模型： *参数* 模型和 *消息协定* 模型。 参数模型使用的托管方法不具有由 <xref:System.ServiceModel.MessageContractAttribute?displayProperty=nameWithType> 类标记的参数或返回值类型。 在此模型中，开发人员控制参数和返回值的序列化，但 WCF 生成用于填充服务及其协定的说明树的值。  
  
 配置文件中指定的绑定直接加载到 <xref:System.ServiceModel.Description.ServiceEndpoint.Binding%2A?displayProperty=nameWithType> 属性中。  
  
|ServiceBehaviorAttribute 属性|受影响的说明树值|  
|---------------------------------------|-------------------------------------|  
|名称|<xref:System.ServiceModel.Description.ServiceDescription.Name%2A>|  
|命名空间|<xref:System.ServiceModel.Description.ServiceDescription.Namespace%2A>|  
|ConfigurationName|<xref:System.ServiceModel.Description.ServiceDescription.ConfigurationName%2A>|  
|IgnoreExtensionDataObject|设置所有操作的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.IgnoreExtensionDataObject%2A> 属性。|  
|MaxItemsInObjectGraph|设置所有操作的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.MaxItemsInObjectGraph%2A> 属性。|  
  
|ServiceContractAttribute 属性|受影响的说明树值|  
|---------------------------------------|-------------------------------------|  
|CallbackContract|添加到所有操作 <xref:System.ServiceModel.Description.ContractDescription.CallbackContractType%2A> 的 <xref:System.ServiceModel.Description.MessageDescription>, <xref:System.ServiceModel.Description.OperationDescription.Messages%2A>。|  
|ConfigurationName|<xref:System.ServiceModel.Description.ContractDescription.ConfigurationName%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.ContractDescription.ProtectionLevel%2A> 和可能的子保护级别。 有关保护级别层次结构的详细信息，请参阅 [了解保护级别](../understanding-protection-level.md)。|  
|SessionMode|<xref:System.ServiceModel.Description.ContractDescription.SessionMode%2A>|  
  
|ServiceKnownTypesAttribute 值|受影响的说明树值|  
|--------------------------------------|-------------------------------------|  
|MethodName|<xref:System.ServiceModel.Description.OperationDescription.KnownTypes%2A>|  
  
|OperationContractAttribute 值|受影响的说明树值|  
|--------------------------------------|-------------------------------------|  
|操作|用于输出消息或输入消息的 <xref:System.ServiceModel.Description.MessageDescription.Action%2A>，具体取决于协定/回调协定。|  
|AsyncPattern|如果为 True，则执行 <xref:System.ServiceModel.Description.OperationDescription.BeginMethod%2A> 和 <xref:System.ServiceModel.Description.OperationDescription.EndMethod%2A>|  
|IsOneWay|映射到 <xref:System.ServiceModel.Description.MessageDescription> 中的单个 <xref:System.ServiceModel.Description.OperationDescription.Messages%2A>|  
|IsInitiating|<xref:System.ServiceModel.Description.OperationDescription.IsInitiating%2A>|  
|IsTerminating|<xref:System.ServiceModel.Description.OperationDescription.IsTerminating%2A>|  
|“属性”|<xref:System.ServiceModel.Description.OperationDescription.Name%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.OperationDescription.ProtectionLevel%2A> 和可能的子保护级别。 有关保护级别层次结构的详细信息，请参阅 [了解保护级别](../understanding-protection-level.md)。|  
|ReplyAction|用于输出消息或输入消息的 <xref:System.ServiceModel.Description.MessageDescription.Action%2A>，具体取决于协定/回调协定。|  
  
|FaultContractAttribute 值|受影响的说明树值|  
|----------------------------------|-------------------------------------|  
|操作|<xref:System.ServiceModel.Description.FaultDescription.Action%2A>，它取决于协定/回调协定。|  
|DetailType|<xref:System.ServiceModel.Description.FaultDescription.DetailType%2A>|  
|名称|<xref:System.ServiceModel.Description.FaultDescription.Name%2A>|  
|命名空间|<xref:System.ServiceModel.Description.FaultDescription.Namespace%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.FaultDescription.ProtectionLevel%2A>|  
  
|DataContractFormatAttribute 值|受影响的说明树值|  
|---------------------------------------|-------------------------------------|  
|使用|<xref:System.ServiceModel.DataContractFormatAttribute.Style%2A> 值是在操作的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 上设置的。|  
  
|XmlSerializerFormatAttribute 值|受影响的说明树值|  
|----------------------------------------|-------------------------------------|  
|Style|此 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 属性是在操作的 <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> 上设置的。|  
|使用|<xref:System.ServiceModel.XmlSerializerFormatAttribute> 是在操作的 <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> 上设置的。|  
  
|TransactionFlowAttribute 值|受影响的说明树值|  
|------------------------------------|-------------------------------------|  
|TransactionFlowOption|<xref:System.ServiceModel.TransactionFlowAttribute> 作为操作行为添加到 <xref:System.ServiceModel.Description.OperationDescription.Behaviors%2A> 属性。|  
  
|MessageContractAttribute 值|受影响的说明树值|  
|------------------------------------|-------------------------------------|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessageDescription.ProtectionLevel%2A>|  
|WrapperName|<xref:System.ServiceModel.Description.MessageBodyDescription.WrapperName%2A>|  
|WrapperNamespace|<xref:System.ServiceModel.Description.MessageBodyDescription.WrapperNamespace%2A>|  
  
|MessageHeaderAttribute 值|受影响的说明树值|  
|----------------------------------|-------------------------------------|  
|Actor|<xref:System.ServiceModel.Description.MessageHeaderDescription.Actor%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|MustUnderstand|<xref:System.ServiceModel.Description.MessageHeaderDescription.MustUnderstand%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|“属性”|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|命名空间|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|中继|<xref:System.ServiceModel.Description.MessageHeaderDescription.Relay%2A> 对于中的相应标头 <xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
  
|MessageBodyMemberAttribute 值|受影响的说明树值|  
|--------------------------------------|-------------------------------------|  
|“属性”|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A> 对于中的相应部分 <xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|命名空间|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A> 对于中的相应部分 <xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|订单|<xref:System.ServiceModel.Description.MessagePartDescription.Index%2A> 对于中的相应部分 <xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A> 对于中的相应部分 <xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
  
|MessageHeaderArrayAttribute 值|受影响的说明树值|  
|---------------------------------------|-------------------------------------|  
|Actor|<xref:System.ServiceModel.Description.MessageHeaderDescription.Actor%2A>|  
|MustUnderstand|<xref:System.ServiceModel.Description.MessageHeaderDescription.MustUnderstand%2A>|  
|名称|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>|  
|命名空间|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A>|  
|中继|<xref:System.ServiceModel.Description.MessageHeaderDescription.Relay%2A>|  
  
|MessagePropertyAttribute 值|受影响的说明树值|  
|------------------------------------|-------------------------------------|  
|“属性”|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>|  
  
|MessageParameterAttribute 值|受影响的说明树值|  
|-------------------------------------|-------------------------------------|  
|“属性”|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A> 对于中的相应部分 <xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
  
 有关如何将说明树值转换为元数据的详细信息，请参阅 [ServiceDescription 和 WSDL 引用](servicedescription-and-wsdl-reference.md)。  
  
## <a name="see-also"></a>另请参阅

- [ServiceDescription 和 WSDL 引用](servicedescription-and-wsdl-reference.md)
