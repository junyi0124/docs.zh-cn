---
title: 服务框架数据
ms.date: 03/30/2017
ms.assetid: 2a2c8ddc-4e82-4e7f-a79f-97085c469517
ms.openlocfilehash: 8bb6e9df6a77eda5875981a0bf7783f50671a589
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255775"
---
# <a name="service-framework-data"></a>服务框架数据

本主题列出由服务框架数据生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|AddressingExtensionInBadNS|指定命名空间中的指定元素无效。 这意味着指定的元素是重复的元素，或者它不是一个合法的扩展，因为扩展元素不能处于寻址命名空间中。|  
|BinaryEncoderSessionInvalid|二进制编码器会话无效，因为解码前一条消息时发生错误。|  
|CannotDetectAddressingVersion|无法检测到 WS-Addressing 版本。 EndpointAddress 不是以元素开头。|  
|CouldNotFindNamespaceForPrefix|指定的前缀在范围中没有命名空间绑定。|  
|EncoderBadContentType|无法处理 contentType。|  
|EncoderEnvelopeVersionMismatch|指定传入消息的信封版本与指定编码器不匹配。 请确保绑定已使用与预期的消息相同的版本进行配置。|  
|EncoderMessageVersionMismatch|指定传出消息的消息版本与指定编码器不匹配。 请确保绑定已使用与消息相同的版本进行配置。|  
|ExtraContentIsPresentInFaultDetail|错误详细信息元素中存在其他可扩展标记语言内容。 仅允许一个元素。|  
|FilterBadTableType|为筛选器创建的 ImessageFilterTable 不能为 MessageFilterTable 或从 MessageFilterTable 派生。|  
|FilterTableInvalidForLookup|MessageFilterTable 处于损坏状态。 无法执行请求的搜索。|  
|MandatoryHeaderNotUnderstood|无法理解一个或多个必需的简单对象访问协议标头块。|  
|MessageBodyIsStream|消息正文是流。|  
|MessageBodyIsUnknown|消息正文的格式未知。|  
|MessageBodyReaderInvalidReadState|无法使用消息正文读取器的指定 ReadState。|  
|MessageTextEncodingNotSupported|不支持文本消息格式中使用的指定文本编码。|  
|MissingMessageID|请求消息缺少 MessageID 标头。 需要 MessageID 标头以与回复相关。|  
|MultipleMessageHeaders|找到多个具有指定名称和命名空间的标头。|  
|MultipleMessageHeadersWithActor|找到多个具有指定名称、命名空间和角色的标头。|  
|MultipleRelatesToHeaders|找到多个具有指定关系的 RelatesTo 标头。 每个关系只允许有一个上述标头。|  
|QueryFunctionTypeNotSupported|不支持 IXsltContextFunction 的指定返回类型。|  
|QueryIteratorOutOfScope|XpathNodeIterator 已失效。 作为参数传递给 IXsltContextFunction 的 XPathNodeIterator 仅在函数中有效。 无法将其缓存供以后使用或由函数返回。|  
|QueryVariableNull|IXsltContextVariable 方法不能返回 null。|  
|QueryVariableTypeNotSupported|不支持指定的 IxsltContextVariable 派生类型。|  
|ReceiveShutdownReturnedMessage|通道在关闭时接收到指定操作的意外输入消息。 在不再需要输入消息时关闭通道。|  
|XmlBufferInInvalidState|发生了内部错误。 由于 XML 缓冲区的状态，无法执行此操作。|  
|XmlBufferQuotaExceeded|缓冲处理可扩展标记语言内容所需的大小超出了缓冲区配额。|
