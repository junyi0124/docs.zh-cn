---
title: XslCompiledTransform 类的输出选项
ms.date: 03/30/2017
ms.assetid: 91ce8cba-386c-411e-bb38-0891a0393c0a
ms.openlocfilehash: 5835cfee69730d5dd2322422aeed6e0d72995eec
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731132"
---
# <a name="output-options-on-the-xslcompiledtransform-class"></a>XslCompiledTransform 类的输出选项

本主题讨论可用的 XSLT 输出选项。 可以在样式表中指定输出选项，或在 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法上指定输出选项。  
  
## <a name="xsloutput-element"></a>xsl:output 元素  

 `xsl:output` 元素指定输出选项。 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法指定的输出类型确定 `xsl:output` 选项的行为。  
  
 下表说明当输出类型为流或 `xsl:output` 时，<xref:System.IO.TextWriter> 元素的每个可用属性的行为。  
  
|特性名|行为|  
|--------------------|--------------|  
|method|支持。|  
|version|已忽略。 对于 XML，版本始终是 1.0，对于 HTML，版本始终是 4.0。|  
|encoding|在输出到 <xref:System.IO.TextWriter> 时忽略。 使用 <xref:System.IO.TextWriter.Encoding%2A?displayProperty=nameWithType> 属性取代。|  
|omit-xml-declaration|支持。|  
|独立|支持。|  
|doctype-public|支持。|  
|doctype-system|支持。|  
|cdata-section-elements|支持。|  
|indent|支持。|  
|media-type|支持。|  
  
#### <a name="sending-output-to-an-xmlwriter"></a>将输出发送到 XmlWriter  

 如果样式表使用 `xsl:output` 元素并且输出类型为 <xref:System.Xml.XmlWriter> 对象，在创建 <xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A?displayProperty=nameWithType> 对象时应使用 <xref:System.Xml.XmlWriter> 属性。 <xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A?displayProperty=nameWithType> 属性返回包含从已编译样式表的 <xref:System.Xml.XmlWriterSettings> 元素派生的信息的 `xsl:output` 对象。 此 <xref:System.Xml.XmlWriterSettings> 对象可以传递给 <xref:System.Xml.XmlWriter.Create%2A?displayProperty=nameWithType> 方法，以使用正确的设置创建 <xref:System.Xml.XmlWriter> 对象。  
  
## <a name="output-types"></a>输出类型  

 下表说明 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 命令的可用输出类型。  
  
#### <a name="xmlwriter"></a>XmlWriter  

 <xref:System.Xml.XmlWriter> 类输出 XML 流或文件。 可以使用 <xref:System.Xml.XmlWriter> 类指定 <xref:System.Xml.XmlWriterSettings> 对象上要支持的功能，包括输出选项。 <xref:System.Xml.XmlWriter> 类是 <xref:System.Xml> 框架必不可少的一个部分。 使用此输出类型可以将输出结果通过管道发送给另一个 XML 进程。  
  
#### <a name="string"></a>String  

 使用此输出类型可以指定输出文件的 URI。  
  
#### <a name="stream"></a>流  

 流是字节序列的抽象，例如文件、输入/输出设备、进程中通信管道或 TCP/IP 套接字。 <xref:System.IO.Stream> 类及其派生类提供这些不同类型的输入和输出的通用视图，使程序员与操作系统和基础设备的具体细节相隔离。  
  
 使用此输出类型可以将数据发送到 <xref:System.IO.FileStream>、<xref:System.IO.MemoryStream> 或输出流 (`Response.OutputStream`)。  
  
#### <a name="textwriter"></a>TextWriter  

 <xref:System.IO.TextWriter> 输出序列字符。 此输出类型在 <xref:System.IO.StringWriter> 和 <xref:System.IO.StreamWriter> 类中实现，分别将字符输出到字符串或流。 如果希望输出到字符串，请使用此输出类型。  
  
## <a name="notes"></a>说明  
  
- 在写出空标记时，会在元素名的最后一个字符与反斜杠之间写入一个空格，例如 `<myElement />`。 这样，较旧的浏览器可以正确地显示生成的 HTML 页面。  
  
## <a name="see-also"></a>请参阅

- [XSLT 转换](xslt-transformations.md)
