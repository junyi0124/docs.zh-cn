---
title: 数据传输和序列化
ms.date: 03/30/2017
helpviewer_keywords:
- data serialization [WCF]
- data transfer [WCF]
ms.assetid: 0f03c635-f3e7-4c5c-9463-3cb0135e221e
ms.openlocfilehash: 490c89f5cfbecd4b2cc0c0e639aa97849132a809
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261977"
---
# <a name="data-transfer-and-serialization"></a>数据传输和序列化

在已连接的系统中，服务和客户端依赖于数据交换来完成任何任务。 作为服务或客户端的开发人员，你还必须了解 Windows Communication Foundation (WCF) 如何处理数据和数据序列化，以便创建高效且易于维护的应用程序。  
  
## <a name="in-this-section"></a>本节内容  

 [在服务协定中指定数据传输](specifying-data-transfer-in-service-contracts.md)  
 描述服务中数据传输的基本概念。  
  
 [使用数据协定](using-data-contracts.md)  
 描述什么是数据协定以及如何创建和使用它们。  
  
 [数据协定序列化程序](data-contract-serializer.md)  
 描述如何通过 <xref:System.Runtime.Serialization.DataContractSerializer> 类或 <xref:System.Runtime.Serialization.XmlObjectSerializer> 类的任何扩展完成数据序列化。  
  
 [使用 XmlSerializer 类](using-the-xmlserializer-class.md)  
 描述如何使用以及为什么使用 <xref:System.Xml.Serialization.XmlSerializer> 类（<xref:System.Runtime.Serialization.DataContractSerializer> 类的替代类）。  
  
 [使用消息约定](using-message-contracts.md)  
 描述消息协定如何允许很好的控制 SOAP 消息。  
  
 [使用 Message 类](using-the-message-class.md)  
 描述如何使用消息类功能。  
  
 [筛选](filtering.md)  
 描述基于各种条件启用消息预处理的筛选。  
  
 [大型数据和流](large-data-and-streaming.md)  
 描述如何发送大数据块，如二进制文件。  
  
 [数据的安全考虑事项](security-considerations-for-data.md)  
 描述在对数据传输和序列化进行编程时要注意的项。  
  
 [数据传输体系结构概述](data-transfer-architectural-overview.md)  
 描述 WCF 中数据传输的总体设计。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel>  
  
 <xref:System.Runtime.Serialization.DataContractSerializer>  
  
 <xref:System.Xml.Serialization.XmlSerializer>  
  
 <xref:System.Runtime.Serialization>  
  
 <xref:System.Xml.Serialization>  
  
## <a name="related-sections"></a>相关章节  

 [扩展编码器和序列化程序](../extending/extending-encoders-and-serializers.md)  
  
## <a name="see-also"></a>另请参阅

- [最佳做法：数据协定版本管理](../best-practices-data-contract-versioning.md)
- [服务版本控制](../service-versioning.md)
