---
title: 如何：以异步方式调用 WCF 服务操作
description: 了解如何使用事件驱动的异步调用模型创建可异步访问服务操作的 WCF 客户端。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0face17f-43ca-417b-9b33-737c0fc360df
ms.openlocfilehash: aa31f64473111800f4cd01907a0446c94f368456
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247229"
---
# <a name="how-to-call-wcf-service-operations-asynchronously"></a>如何：以异步方式调用 WCF 服务操作

本文介绍客户端如何以异步方式访问服务操作。 本文中的服务实现 `ICalculator` 接口。 通过使用事件驱动的异步调用模型，客户端可以对此接口异步调用操作。 （有关基于事件的异步调用模型的详细信息，请参阅[采用基于事件的异步模式进行多线程编程](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)）。 有关演示如何在服务中异步实现操作的示例，请参阅[如何：实现异步服务操作](../how-to-implement-an-asynchronous-service-operation.md)。 有关同步和异步操作的详细信息，请参阅[同步和异步操作](../synchronous-and-asynchronous-operations.md)。  
  
> [!NOTE]
> 使用 <xref:System.ServiceModel.ChannelFactory%601> 时，不支持事件驱动的异步调用模型。 有关使用进行异步调用的信息 <xref:System.ServiceModel.ChannelFactory%601> ，请参阅[如何：使用通道工厂以异步方式调用操作](how-to-call-operations-asynchronously-using-a-channel-factory.md)。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-call-wcf-service-operations-asynchronously"></a>以异步方式调用 WCF 服务操作  
  
1. 同时运行带的[元数据实用工具（Svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)工具 `/async` 和 `/tcv:Version35` 命令选项，如下面的命令中所示。  
  
    ```console
    svcutil /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost:8000/servicemodelsamples/service/mex /a /tcv:Version35  
    ```  
  
     这除了同步和标准的基于委托的异步操作，还会生成一个包含以下内容的 WCF 客户端类：  
  
    - 两个 <`operationName` > `Async` 操作，用于基于事件的异步调用方法。 例如：  
  
         [!code-csharp[EventAsync#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#1)]
         [!code-vb[EventAsync#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#1)]  
  
    - <窗体的操作完成事件 `operationName` > `Completed` 与基于事件的异步调用方法一起使用。 例如：  
  
         [!code-csharp[EventAsync#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#2)]
         [!code-vb[EventAsync#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#2)]  
  
    - <xref:System.EventArgs?displayProperty=nameWithType>每个操作的类型（窗体 <`operationName` > `CompletedEventArgs` ）用于基于事件的异步调用方法。 例如：  
  
         [!code-csharp[EventAsync#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#3)]
         [!code-vb[EventAsync#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#3)]  
  
2. 在调用应用程序中，创建一个要在异步操作完成时调用的回调方法，如下面的示例代码所示。  
  
     [!code-csharp[EventAsync#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#4)]
     [!code-vb[EventAsync#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#4)]  
  
3. 在调用操作之前，请使用类型 <的新泛型， <xref:System.EventHandler%601?displayProperty=nameWithType> `operationName` > `EventArgs` 将处理程序方法（在上一步中创建）添加到 <`operationName` > `Completed` 事件。 然后调用 <`operationName` > `Async` 方法。 例如：  
  
     [!code-csharp[EventAsync#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#5)]
     [!code-vb[EventAsync#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#5)]  
  
## <a name="example"></a>示例  
  
> [!NOTE]
> 基于事件的异步模型设计准则规定，如果返回了多个值，则一个值会作为 `Result` 属性返回，其他值会作为 <xref:System.EventArgs> 对象上的属性返回。 因此产生的结果之一是，如果客户端使用基于事件的异步命令选项导入元数据，且该操作返回多个值，则默认的 <xref:System.EventArgs> 对象返回一个值作为 `Result` 属性，返回的其余值是 <xref:System.EventArgs> 对象的属性。如果要将消息对象作为 `Result` 属性来接收并要使返回的值作为该对象上的属性，请使用 `/messageContract` 命令选项。 这会生成一个签名，该签名会将响应消息作为 `Result` 对象上的 <xref:System.EventArgs> 属性返回。 然后，所有内部返回值就都是响应消息对象的属性了。  
  
 [!code-csharp[EventAsync#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#6)]
 [!code-vb[EventAsync#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#6)]  
  
## <a name="see-also"></a>请参阅

- [如何：实现异步服务操作](../how-to-implement-an-asynchronous-service-operation.md)
