---
title: 发布元数据
description: 了解 WCF 服务如何通过发布一个或多个元数据终结点来发布元数据，从而使元数据可通过标准协议使用。
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], publishing
ms.assetid: 3a56831a-cabc-45c0-bd02-12e2e9bd7313
ms.openlocfilehash: 3ea2377870a37e7dee6f4253503d70167d021a0d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244725"
---
# <a name="publishing-metadata"></a>发布元数据

Windows Communication Foundation (WCF) 服务通过发布一个或多个元数据终结点来发布元数据。 发布服务元数据之后，可以通过标准协议（如 WS-MetadataExchange (MEX) 和 HTTP/GET 请求）来使用该元数据。 元数据终结点类似于其他服务终结点，因为它们都有一个地址、一个绑定和一个协定，并且它们都可通过配置或命令代码添加到服务主机。  
  
## <a name="publishing-metadata-endpoints"></a>发布元数据终结点  

 若要发布 WCF 服务的元数据终结点，你必须首先将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 服务行为添加到服务。 添加一个 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 实例将允许服务公开元数据终结点。 添加 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 服务行为之后，就可以公开支持 MEX 协议或响应 HTTP/GET 请求的元数据终结点。  
  
 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 使用 <xref:System.ServiceModel.Description.WsdlExporter> 来导出服务中所有服务终结点的元数据。 有关从服务中导出元数据的详细信息，请参阅 [导出和导入元数据](exporting-and-importing-metadata.md)。  
  
 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 添加一个 <xref:System.ServiceModel.Description.ServiceMetadataExtension> 实例作为服务主机的扩展。 <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> 提供了元数据发布协议的实现。 还可以使用 <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> 通过访问 <xref:System.ServiceModel.Description.ServiceMetadataExtension.Metadata%2A?displayProperty=nameWithType> 属性来在运行时获取服务的元数据。  
  
### <a name="mex-metadata-endpoints"></a>MEX 元数据终结点  

 要添加使用 MEX 协议的元数据终结点，请将服务终结点添加到使用 `IMetadataExchange` 服务协定的服务主机。 WCF 包含一个 <xref:System.ServiceModel.Description.IMetadataExchange> 接口，该接口具有此服务协定名称，你可以将其用作 WCF 编程模型的一部分。 WS-MetadataExchange 终结点或 MEX 终结点，可以使用静态工厂方法在类上公开的四个默认绑定之一， <xref:System.ServiceModel.Description.MetadataExchangeBindings> 以匹配 WCF 工具（如 Svcutil.exe）所使用的默认绑定。 还可以使用自己的自定义绑定来配置 MEX 元数据终结点。  
  
### <a name="http-get-metadata-endpoints"></a>HTTP GET 元数据终结点  

 若要将元数据终结点添加到响应 HTTP/GET 请求的服务，请将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A> 的 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 属性设置为 `true`。 将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpsGetEnabled%2A> 的 <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> 属性设置为 `true` 还可以配置使用 HTTPS 的元数据终结点。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：使用配置文件发布服务的元数据](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)  
 演示如何将 WCF 服务配置为发布元数据，以便客户端可以使用 WS-MetadataExchange 或使用查询字符串的 HTTP/GET 请求检索元数据 `?wsdl` 。  
  
 [如何：使用代码发布服务的元数据](how-to-publish-metadata-for-a-service-using-code.md)  
 演示如何在代码中为 WCF 服务启用元数据发布，以便客户端可以使用 WS-MetadataExchange 或使用查询字符串的 HTTP/GET 请求来检索元数据 `?wsdl` 。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel.Description.ServiceMetadataBehavior>  
  
 <xref:System.ServiceModel.Description.IMetadataExchange>  
  
 <xref:System.ServiceModel.Description.ServiceMetadataExtension>  
  
 <xref:System.ServiceModel.Description.MetadataExchangeBindings>  
  
## <a name="see-also"></a>请参阅

- [导出和导入元数据](exporting-and-importing-metadata.md)
