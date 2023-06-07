---
title: 如何：对序列化数据进行分块
description: 可以对数据进行分块，以避免大型数据集出现问题。 实现 IXmlSerializable 接口来控制序列化和反序列化。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- chunking serialized data
- data chunking
- binary serialization, chunking data
- large data set chunking
- serialization, chunking data
- serialization, examples
- binary serialization, examples
ms.assetid: 22f1b818-7e0d-428a-8680-f17d6ebdd185
ms.openlocfilehash: ee8bab4fe7659b1fe5b7edeabc81187d0a13e5bc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678995"
---
# <a name="how-to-chunk-serialized-data"></a>如何：对序列化数据进行分块

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

在 Web 服务消息中发送大型数据集时，会出现下面两个问题：  
  
1. 因序列化引擎执行缓冲而使工作集（内存）很大。  
  
2. 因使用 Base64 编码后产生 33% 的膨胀而过度占用带宽。  
  
 若要解决这两个问题，可以通过实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口，来控制序列化和反序列化。 具体来讲，就是实现 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 和 <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> 方法，对数据进行分块。  
  
### <a name="to-implement-server-side-chunking"></a>实现服务器端数据分块  
  
1. 在服务器计算机上，必须采用 Web 方法禁用 ASP.NET 缓冲，并返回实现 <xref:System.Xml.Serialization.IXmlSerializable> 的类型。  
  
2. 实现 <xref:System.Xml.Serialization.IXmlSerializable> 的类型可以采用 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 方法对数据进行分块。  
  
### <a name="to-implement-client-side-processing"></a>实现客户端处理  
  
1. 通过更改客户端代理上的 Web 方法，可以返回实现 <xref:System.Xml.Serialization.IXmlSerializable> 的类型。 可以使用 <xref:System.Xml.Serialization.Advanced.SchemaImporterExtension> 自动执行这项操作，但此处不加以说明。  
  
2. 通过实现 <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> 方法，可以读取分块数据流，并将字节写入磁盘。 此实现还会引发可供图形控件（如进度栏）使用的进度事件。  
  
## <a name="example"></a>示例  

下面的代码示例演示客户端上用于禁用 ASP.NET 缓冲的 Web 方法。 它还演示了如何在客户端实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口，该实现可以采用 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 方法对数据进行分块。  
  
[!code-csharp[HowToChunkSerializedData#1](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#1)]
[!code-vb[HowToChunkSerializedData#1](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#1)]  
[!code-csharp[HowToChunkSerializedData#2](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#2)]
[!code-vb[HowToChunkSerializedData#2](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#2)]  
[!code-csharp[HowToChunkSerializedData#3](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#3)]
[!code-vb[HowToChunkSerializedData#3](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#3)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 代码使用下面的命名空间：<xref:System>、<xref:System.Runtime.Serialization>、<xref:System.Web.Services>、<xref:System.Web.Services.Protocols>、<xref:System.Xml>、<xref:System.Xml.Serialization> 和 <xref:System.Xml.Schema>。  
  
## <a name="see-also"></a>请参阅

- [自定义序列化](custom-serialization.md)
