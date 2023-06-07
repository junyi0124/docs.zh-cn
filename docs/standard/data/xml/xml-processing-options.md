---
title: XML 处理选项
description: 查看 XML 处理选项，其中包括 LINQ to XML、XmlReader、XmlWriter、XmlDocument、XPathNavigator、XslCompiledTransform、XmlLite 和 MSXML。
ms.date: 03/30/2017
ms.assetid: 33ced8ee-1745-4e71-8dee-ebe70ec067c7
ms.openlocfilehash: 4388b815625a7ca4b26c35518fa4a58a97bc67d5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733030"
---
# <a name="xml-processing-options"></a>XML 处理选项

请参见下面的表以获得可用于处理 XML 数据的 Microsoft 技术的列表。  
  
## <a name="net-framework-options"></a>.NET Framework 选项  
  
|**选项**|**处理类型**|**说明**|  
|----------------|-------------------------|---------------------|  
|[LINQ to XML (C#)](../../linq/linq-xml-overview.md) <br/> [LINQ to XML (Visual Basic)](../../linq/linq-xml-overview.md) <br />（<xref:System.Xml.Linq> 命名空间）|内存中|-   依据为 .NET Framework 语言集成查询 (LINQ) 技术。<br />-   提供与 SQL 类似的对象、关系数据和 XML 数据查询体验。<br />-   提供直观的文档创建和转换功能。<br />-   若要编写新代码，请使用此选项。|  
|<xref:System.Xml.XmlReader?displayProperty=nameWithType>|基于流|-   提供用于访问 XML 数据的非缓存、仅正向的快速方法。<br />-   可以使用 <xref:System.Xml.XmlReader.Create%2A?displayProperty=nameWithType> 方法创建对象，并使用 <xref:System.Xml.XmlReaderSettings> 类指定对对象启用的一组功能。|  
|<xref:System.Xml.XmlWriter?displayProperty=nameWithType>|基于流|-   提供用于生成 XML 数据的非缓存、仅正向的快速方法。<br />-   可以使用 <xref:System.Xml.XmlWriter.Create%2A?displayProperty=nameWithType> 方法创建对象，并使用 <xref:System.Xml.XmlWriterSettings> 类指定对对象启用的一组功能。|  
|<xref:System.Xml.XmlDocument?displayProperty=nameWithType>|内存中|-   实现 [W3C 文档对象模型 (DOM) 级别 1 核心](https://www.w3.org/TR/REC-DOM-Level-1/level-one-core.html)和 [DOM 级别 2 核心](https://www.w3.org/TR/DOM-Level-2-Core/)建议。<br />-   可以使用基于熟悉 DOM 模型的方法和属性，创建、插入、删除和修改节点。<br />-   若要修改利用 W3C DOM 的现有代码，请使用此选项。|  
|<xref:System.Xml.XPath.XPathNavigator?displayProperty=nameWithType>|内存中|-   使用游标模型提供多个编辑选项和导航功能。<br />-   XML 文档可以包含在 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中。<br />-   提供出色性能，以便于只读处理 XML。<br />-   若要通过 XPath 查询或 XSLT 转换来修改现有代码，请使用此选项。|  
|<xref:System.Xml.Xsl.XslCompiledTransform>|内存中|-   提供通过 XSL 转换来转换 XML 数据的选项。<br />-   通过 [XSLT 编译器 (xsltc.exe)](xslt-compiler-xsltc-exe.md)，可以在应用中引用预编译转换。|  
  
## <a name="win32-and-com-based-options"></a>基于 Win32 和 COM 的选项  
  
|**选项**|**说明**|  
|----------------|---------------------|  
|[XmlLite](/previous-versions/windows/desktop/ms752872(v=vs.85))|-   快速安全、非缓存、仅正向 XML 分析器，有助于生成高性能 XML 应用。<br />-   支持能够使用动态链接库 (DLL) 的任何语言；建议使用 C++。|  
|[MSXML](/previous-versions/windows/desktop/ms763742(v=vs.85))|-   基于 COM 的技术，用于处理 Windows 操作系统随附的 XML。<br />-   提供 DOM 本机实现，同时支持 XPath 和 XSLT。<br />-   包含 SAX2 基于事件的分析器。|  
  
## <a name="see-also"></a>请参阅

- [使用 DOM 模型处理 XML 数据](process-xml-data-using-the-dom-model.md)
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [XSLT 编译器 (xsltc.exe)](xslt-compiler-xsltc-exe.md)
