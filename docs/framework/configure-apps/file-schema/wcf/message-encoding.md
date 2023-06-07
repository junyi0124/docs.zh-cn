---
title: 消息编码
ms.date: 03/30/2017
ms.assetid: f30ee941-aca9-4c67-82a5-421568496f07
ms.openlocfilehash: 018cbc778627fc429e443fc590fa4c0f52d2a68a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204874"
---
# <a name="message-encoding"></a>消息编码

编码是将一组 Unicode 字符转换为一个字节序列的过程。 解码是反向过程。 Windows Communication Foundation (WCF) 包含三种类型的 SOAP 消息编码：文本、二进制和消息传输优化机制 (MTOM)。  
  
 `binaryMessageEncoding` 配置节指定基于二进制的 XML 消息所使用的字符编码和消息版本管理。 二进制消息编码器在网络上以二进制形式对 Windows Communication Foundation (WCF) 消息进行编码。 虽然这种编码有助于非常快速地传输消息，但是丢失了基于 WS-* 标准的互操作性。  
  
 `mtomMessageEncoding` 配置节指定用于使用消息传输优化机制 (MTOM) 编码的消息的字符编码和消息版本管理。 (MTOM) 是一种用于传输 Windows Communication Foundation (WCF) 消息中二进制数据的有效技术。 MTOM 编码器试图在效率和互操作性之间取得平衡。 MTOM 编码以文本形式传输大多数 XML，但是会通过按原样（即不转换为文本）的方式传输来优化大型二进制数据块。  
  
 `textMessageEncoding` 配置节指定用于在网络上创建基于文本的消息的文本编码器。 此编码器产生的消息适合于基于 WS-* 的互操作性。 Web 服务或 Web 服务客户端通常可以理解文本 XML。 但是，对于 XML 消息编码来说，以文本形式传输较大的二进制数据块是最低效的方法。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [选择消息编码器](../../../wcf/feature-details/choosing-a-message-encoder.md)
