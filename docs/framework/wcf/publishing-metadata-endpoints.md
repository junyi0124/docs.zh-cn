---
title: 发布元数据终结点
ms.date: 03/30/2017
ms.assetid: 29cd8a60-dfb7-460c-bf5a-c2b31b782671
ms.openlocfilehash: 5be27a72c87d95e36a5b283e7930ba0a5191a5a1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249756"
---
# <a name="publishing-metadata-endpoints"></a>发布元数据终结点

Windows Communication Foundation (WCF) 服务通过发布一个或多个元数据终结点来发布元数据。 发布服务元数据之后，可以通过标准协议（如 WS-MetadataExchange (MEX) 和 HTTP/GET 请求）来使用该元数据。 元数据终结点类似于其他服务终结点：它们都有一个地址、一个绑定和一个协定，并且它们都可通过配置或使用代码添加到服务主机。 若要启用发布元数据终结点，必须将 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 服务行为添加到该服务。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：使用配置文件发布服务的元数据](./feature-details/how-to-publish-metadata-for-a-service-using-a-configuration-file.md)  
 演示如何将 WCF 服务配置为发布元数据，以便客户端可以使用 WS-MetadataExchange 或使用查询字符串的 HTTP/GET 请求检索元数据 `?wsdl` 。  
  
 [如何：使用代码发布服务的元数据](./feature-details/how-to-publish-metadata-for-a-service-using-code.md)  
 演示如何在代码中为 WCF 服务启用元数据发布，以便客户端可以使用 WS-MetadataExchange 或使用查询字符串的 HTTP/GET 请求来检索元数据 `?wsdl` 。  
  
## <a name="see-also"></a>另请参阅

- [发布元数据](./feature-details/publishing-metadata.md)
