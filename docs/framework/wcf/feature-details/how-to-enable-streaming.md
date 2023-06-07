---
title: 如何：启用流处理
description: 了解如何在 WCF 中启用流消息而不是默认的缓冲传输，这些传输消息必须在处理前完全接收。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6ca2cf4b-c7a1-49d8-a79b-843a90556ba4
ms.openlocfilehash: 7f77c406e1cfd4def116a1abe24aa92be74c6abe
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237737"
---
# <a name="how-to-enable-streaming"></a>如何：启用流处理

Windows Communication Foundation (WCF) 可以使用缓冲传输或流式传输发送消息。 在默认的缓冲传输模式中，只有在一条消息全部传递完之后，接收方才能读取该消息。 在流传输模式中，不必等到消息全部传递完，接收方便可以开始处理该消息。 当传递的信息很长且可以依次处理时，流模式非常有用。 当消息过长以致于无法全部缓冲时，流模式也非常有用。  
  
 若要启用流处理，请适当地定义 `OperationContract` 并在传输级别上启用流处理。  
  
### <a name="to-stream-data"></a>对数据进行流处理  
  
1. 若要对数据进行流处理，服务的 `OperationContract` 必须满足两个要求：  
  
    1. 保留要进行流处理的数据的参数必须是方法中的唯一参数。 例如，如果要对输入消息进行流处理，则该操作必须正好具有一个输入参数。 同样，如果要对输出消息进行流处理，则该操作必须正好具有一个输出参数或一个返回值。  
  
    2. 参数和返回值的类型中至少有一个必须是 <xref:System.IO.Stream>, <xref:System.ServiceModel.Channels.Message> 或 <xref:System.Xml.Serialization.IXmlSerializable>。  
  
     下面是流处理数据的协定的示例。  
  
     [!code-csharp[c_HowTo_EnableStreaming#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#1)]
     [!code-vb[c_HowTo_EnableStreaming#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#1)]  
  
     `GetStream` 操作接收一些缓冲输入数据作为经过缓冲的 `string`，并返回经过流处理的 `Stream`。 相反，`UploadStream` 接收一个经过流处理的 `Stream`，并返回一个经过缓冲的 `bool`。 `EchoStream` 是输入消息和输出消息都经过流处理的操作的示例，它接收和返回 `Stream`。 最后，`GetReversedStream` 将不接收输入，并返回一个 `Stream`（已经过流处理）。  
  
2. 必须在绑定上启用流处理。 设置 `TransferMode` 属性，可以采用下面的值之一：  
  
    1. `Buffered`,  
  
    2. `Streamed`，此值在两个方向上启用流通信。  
  
    3. `StreamedRequest`，此值仅启用请求流处理。  
  
    4. `StreamedResponse`，此值仅启用响应流处理。  
  
     `BasicHttpBinding` 公开绑定上的 `TransferMode` 属性，与 `NetTcpBinding` 和 `NetNamedPipeBinding` 一样。 还可以在传输绑定元素上设置 `TransferMode` 属性，并且在自定义绑定中使用。  
  
     下面的示例演示如何通过代码和通过更改配置文件来设置 `TransferMode`。 两个示例都将 `maxReceivedMessageSize` 属性设置为 64 MB，这是可以接收的最大消息大小。 默认的 `maxReceivedMessageSize` 为 64 KB，对于流处理方案而言，此值通常太低。 根据应用程序期望接收的消息的最大大小适当地设置此配额设置。 还请注意，`maxBufferSize` 可控制缓冲的最大大小，应适当地进行设置。  
  
    1. 下面示例中的配置段演示如何将 `TransferMode` 和自定义 HTTP 绑定上的 `basicHttpBinding` 属性设置为流处理。  
  
         [!code-xml[c_HowTo_EnableStreaming#103](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/common/app.config#103)]
  
    2. 下面的代码段演示如何将 `TransferMode` 和自定义 HTTP 绑定上的 `basicHttpBinding` 属性设置为流处理。  
  
         [!code-csharp[c_HowTo_EnableStreaming_code#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming_code/cs/c_howto_enablestreaming_code.cs#2)]
         [!code-vb[c_HowTo_EnableStreaming_code#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming_code/vb/c_howto_enablestreaming_code.vb#2)]  
  
    3. 下面的代码段演示如何将自定义 TCP 绑定上的 `TransferMode` 属性设置为流处理。  
  
         [!code-csharp[c_HowTo_EnableStreaming_code#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming_code/cs/c_howto_enablestreaming_code.cs#3)]
         [!code-vb[c_HowTo_EnableStreaming_code#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming_code/vb/c_howto_enablestreaming_code.vb#3)]  
  
3. `GetStream`、`UploadStream` 和 `EchoStream` 操作都可以处理直接从文件发送数据或将接收的数据直接保存到文件的情况。 下面的代码适用于 `GetStream`。  
  
     [!code-csharp[c_HowTo_EnableStreaming#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#4)]
     [!code-vb[c_HowTo_EnableStreaming#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#4)]  
  
### <a name="writing-a-custom-stream"></a>编写自定义流  
  
1. 若要在发送或接收数据流的每个块区时对其进行特殊处理，可从 <xref:System.IO.Stream> 派生一个自定义流类。 与自定义流的示例一样，下面的代码包含 `GetReversedStream` 方法和 `ReverseStream` 类。  
  
     `GetReversedStream` 创建并返回 `ReverseStream` 的新实例。 当系统从 `ReverseStream` 对象中读取时，发生实际处理。 `ReverseStream.Read` 方法从基础文件中读取字节块区，反转字节，然后返回反转的字节。 此方法不会反转整个文件内容；一次只能反转一个字节块区。 本示例演示在从流中读取内容或将内容写入到流中时如何执行流处理。  
  
     [!code-csharp[c_HowTo_EnableStreaming#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_enablestreaming/cs/service.cs#2)]
     [!code-vb[c_HowTo_EnableStreaming#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_enablestreaming/vb/service.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [大型数据和流](large-data-and-streaming.md)
- [Stream](../samples/stream.md)
