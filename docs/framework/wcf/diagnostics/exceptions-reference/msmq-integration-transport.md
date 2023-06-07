---
title: MSMQ 集成传输
ms.date: 03/30/2017
ms.assetid: 2bf9893a-fbd1-41fc-b6de-a41a44279936
ms.openlocfilehash: 032b6886152fb73c382fed1572e8c184a822b44f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248742"
---
# <a name="msmq-integration-transport"></a>MSMQ 集成传输

本主题列出由 MSMQ 集成传输生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|MessageSizeMustBeInIntegerRange|此工厂会对消息进行缓冲处理，因此消息大小必须在整数值的范围之内。|  
|MsmqByteArrayBodyExpected|指定的序列化格式与 MSMQ 消息正文不匹配。 无法发送或接收消息。 序列化格式 ByteArray 要求 MSMQ 消息的正文类型为 byte[]。|  
|MsmqCannotDeserializeActiveXMessage|发生 ActiveX 序列化错误。 无法发送或接收消息。 为正文指定的变量类型与实际 MSMQ 消息正文不匹配。|  
|MsmqCannotUseBodyTypeWithActiveXSerialization|消息的属性不匹配。 无法发送或接收消息。 如果使用 ActiveX 序列化格式，则不能指定 BodyType 消息属性。|  
|MsmqInvalidServiceOperationForMsmqIntegrationBinding|MsmqIntegrationBinding 验证失败。 无法启动服务终结点。 指定绑定不支持指定协定中指定服务操作的方法签名。 更正服务操作以使用 MsmqIntegrationBinding。|  
|MsmqInvalidTypeDeserialization|ActiveX 序列化失败，因为无法识别序列化格式。 无法发送或接收消息。|  
|MsmqInvalidTypeSerialization|无法识别变量类型。 ActiveX 序列化失败。 无法发送或接收消息。 指定的变量类型不受支持。|  
|MsmqStreamBodyExpected|序列化格式与正文内容不匹配。 无法发送或接收消息。 使用流序列化模式只能发送或接收类型为 Stream 的正文。|
