---
title: 使用字节流编码器编码二进制对象
ms.date: 03/30/2017
ms.assetid: 020ee981-c889-4b12-a3ea-91823ef46444
ms.openlocfilehash: cc63cb8de1e245c3b58fb69819e59cb815b777d3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276732"
---
# <a name="encoding-binary-objects-with-bytestream-encoder"></a>使用字节流编码器编码二进制对象

使用 Windows Communication Foundation (WCF) 发送和接收原始二进制数据 <xref:System.ServiceModel.Channels.ByteStreamMessageEncodingBindingElement> 。  
  
## <a name="byte-stream-message-encoder-architecture"></a>字节流消息编码器体系结构  

 WCF 使用的二进制消息编码器没有用于处理、验证或标识消息中的基础二进制数据的工具。 数据包会编码为 XML 进行发送、接收和解码。 编码器在数据传递到传输之后但在消息发送到消息队列之前处理数据。 在功能上，二进制编码器将消息数据包装在 `<binary>` 元素中进行发送，并在收到消息之后删除元素。  
  
## <a name="using-the-byte-stream-message-encoder"></a>使用字节流消息编码器  

 下面的示例演示实现字节流消息编码器的服务协定。  
  
```csharp  
[OperationContract]  
Void Myfunction(Stream stream);  
```  
  
 下面的示例演示调用的服务。  
  
```csharp  
proxy.MyFunction(stream);  
```  
  
 使用实现消息基础结构（如路由器）的服务时，将直接处理消息，而不进行检查、验证或通过其他方式与消息交互，如下面的示例所示。  
  
```csharp  
[OperationContract]  
void ProcessMessage(Message message) ;  
```  
  
## <a name="scenarios"></a>方案  

 字节流编码器可在以下方案中使用。  
  
- 使用 WCF 在计算机之间传输 JPEG 图像。 在此方案中，图像会通过传输从外部源到达，而发送的数据会是组成图像的原始字节。 某个服务会接收二进制数据并显示图像。  
  
- 从消息队列读出信息并进行处理。 消息会从消息队列管理器中读取，并在消息队列通道中向上传递以进行处理。 消息队列通道将充当 WCF 信道堆栈中的队列管理器。  
  
 对于通过消息队列通道发送消息，发送方无法控制从队列管理器接收的字节。 如果接收进程无法读取原始字节，则消息会以错误格式接收，不会进行处理；假定接收进程能够将接收的字节转换为可用格式。
