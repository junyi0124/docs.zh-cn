---
title: 如何：使用 MetadataExchangeClient 检索元数据
ms.date: 03/30/2017
ms.assetid: 0754e9dc-13c5-45c2-81b5-f3da466e5a87
ms.openlocfilehash: 412e695036f29886310099cc5a55b940247b0c72
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280827"
---
# <a name="how-to-use-metadataexchangeclient-to-retrieve-metadata"></a>如何：使用 MetadataExchangeClient 检索元数据

使用 <xref:System.ServiceModel.Description.MetadataExchangeClient> 类可以下载使用 WS-MetadataExchange (MEX) 协议的元数据。 检索到的元数据文件作为 <xref:System.ServiceModel.Description.MetadataSet> 对象返回。 返回的 <xref:System.ServiceModel.Description.MetadataSet> 对象包含 <xref:System.ServiceModel.Description.MetadataSection> 对象的集合，其中每个对象包含一个特定的元数据方言和一个标识符。 你可以将返回的元数据写入文件；或者，如果返回的元数据包含 Web 服务描述语言 (WSDL) 文档，则可以使用 <xref:System.ServiceModel.Description.WsdlImporter> 导入元数据。  
  
 接受地址的 <xref:System.ServiceModel.Description.MetadataExchangeClient> 构造函数可对符合该地址的统一资源标识符 (URI) 方案的 <xref:System.ServiceModel.Description.MetadataExchangeBindings> 静态类使用绑定。 你也可以使用 <xref:System.ServiceModel.Description.MetadataExchangeClient> 构造函数显式指定要使用的绑定。 指定的绑定用于解析所有元数据引用。  
  
 就像任何其他 Windows Communication Foundation (WCF) 客户端一样，该 <xref:System.ServiceModel.Description.MetadataExchangeClient> 类型还提供了一个构造函数，用于加载使用终结点配置名称的客户端终结点配置。 指定的终结点配置必须指定 <xref:System.ServiceModel.Description.IMetadataExchange> 约定。 不会加载终结点配置中的地址，因此必须使用接受地址的 <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> 重载之一。 当使用 <xref:System.ServiceModel.EndpointAddress> 实例指定元数据地址时，<xref:System.ServiceModel.Description.MetadataExchangeClient> 假设该地址指向 MEX 终结点。 如果将元数据地址指定为 URL，则您还需要指定要使用的 <xref:System.ServiceModel.Description.MetadataExchangeClientMode>（MEX 或 HTTP GET）。  
  
> [!IMPORTANT]
> 默认情况下，<xref:System.ServiceModel.Description.MetadataExchangeClient> 为您解析所有引用，包括 WSDL 和 XML 架构导入和包括的内容。 通过将 <xref:System.ServiceModel.Description.MetadataExchangeClient.ResolveMetadataReferences%2A> 属性设置为 `false`，可以禁用此功能。 使用 <xref:System.ServiceModel.Description.MetadataExchangeClient.MaximumResolvedReferences%2A> 属性可以控制要解析的最大引用数。 可以与 `MaxReceivedMessageSize` 属性一起对绑定使用此属性，以控制所检索的元数据的量。  
  
### <a name="to-use-metadataexchangeclient-to-obtain-metadata"></a>使用 MetadataExchangeClient 获取元数据  
  
1. 通过显式指定一个绑定、一个终结点配置名称或元数据的地址，创建一个新的 <xref:System.ServiceModel.Description.MetadataExchangeClient> 对象。  
  
2. 配置 <xref:System.ServiceModel.Description.MetadataExchangeClient> 以适合您的需要。 例如，您可以指定请求元数据时要使用的凭据、控制元数据引用的解析方式和设置 <xref:System.ServiceModel.Description.MetadataExchangeClient.OperationTimeout%2A> 属性以控制元数据请求超时之前必须在多长时间内返回。  
  
3. 通过调用 <xref:System.ServiceModel.Description.MetadataSet> 方法之一获取包含检索的元数据的 <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> 对象。 请注意，如果你在构造 <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> 时显式指定了地址，则只能使用不带自变量的 <xref:System.ServiceModel.Description.MetadataExchangeClient> 重载。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何使用 <xref:System.ServiceModel.Description.MetadataExchangeClient> 下载和枚举元数据文件。  

 [!code-csharp[MetadataResolver#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/metadataresolver/cs/client.cs#3)]  

## <a name="compiling-the-code"></a>编译代码  

 若要编译此代码示例，必须引用 System.ServiceModel.dll 程序集并导入 <xref:System.ServiceModel.Description> 命名空间。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.MetadataResolver>
- <xref:System.ServiceModel.Description.MetadataExchangeClient>
- <xref:System.ServiceModel.Description.WsdlImporter>
