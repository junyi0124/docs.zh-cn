---
title: <mtomMessageEncoding>
ms.date: 03/30/2017
ms.assetid: 7865d171-cd1e-430a-8421-39cc13541d1b
ms.openlocfilehash: 76b83381849b8519c1b758ef52c6d5c3f682f9b7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204627"
---
# \<mtomMessageEncoding>

指定用于基于 SOAP 消息传输优化机制 (MTOM) 的消息的编码和消息版本控制。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<mtomMessageEncoding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<mtomMessageEncoding maxBufferSize="Integer"
                     maxReadPoolSize="Integer"
                     maxWritePoolSize="Integer"
                     messageVersion="Soap11Addressing1/Soap12Addressing10"
                     writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|maxBufferSize|一个指定可以使用的缓冲区最大大小的整数。|  
|maxReadPoolSize|一个整数，指定在无需分配新读取器的情况下可以同时读取的消息数。 池越大，系统允许的活动峰值就越大，但工作集也会随之增大。 默认值为 64。|  
|maxWritePoolSize|一个整数，指定在无需分配新编写器的情况下可以同时发送的消息数。 池越大，系统允许的活动峰值就越大，但工作集也会随之增大。 默认值为 16。|  
|messageVersion|指定使用此绑定发送的消息的 SOAP 版本。 有效值为<br /><br /> - Soap11Addressing1<br />- Soap12Addressing10<br /><br /> 默认值为 Soap12Addressing10。 此属性的类型为 <xref:System.ServiceModel.Channels.MessageVersion>。|  
|writeEncoding|指定要用来在绑定上发出消息的字符集编码。 有效值为<br /><br /> -UnicodeFffeTextEncoding： Unicode BigEndian 编码<br />-Utf16TextEncoding： Unicode 编码<br />-Utf8TextEncoding：8位编码<br /><br /> 默认值为 Utf8TextEncoding。 此属性的类型为 <xref:System.Text.Encoding>。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义自定义绑定的所有绑定功能。|  
  
## <a name="remarks"></a>备注  

 编码是将消息转换为一个字节序列的过程。 解码是反向过程。 Windows Communication Foundation (WCF) 包含三种类型的 SOAP 消息编码：文本、二进制和消息传输优化机制 (MTOM)。  
  
 `MtomMessageEncoding` 元素指定使用消息传输优化机制 (MTOM) 编码的消息所用的字符编码和消息版本管理以及其他设置。 MTOM 是一种用于在 WCF 消息中传输二进制数据的有效技术。 MTOM 编码器会尝试在效率和互操作性之间建立平衡。 MTOM 编码以文本形式传输大多数 XML，但通过按原样传输来优化大型二进制数据块的传输，无需将其转换为 base64 编码格式。  
  
## <a name="example"></a>示例  
  
```xml  
<mtomMessageEncoding maxReadPoolSize="211"
                     maxWritePoolSize="2132"
                     messageVersion="Soap11Addressing10"
                     textEncoding="utf-8" />
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.MtomMessageEncodingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>
- [消息编码](message-encoding.md)
- [选择消息编码器](../../../wcf/feature-details/choosing-a-message-encoder.md)
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
