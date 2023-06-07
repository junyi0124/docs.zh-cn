---
title: 将 XML 文档读入 DOM
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a4fb291f-5630-49ba-a49a-5b66c3b71e49
ms.openlocfilehash: 61275e9232b3d9e516636869d7153f33133cbd03
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686860"
---
# <a name="reading-an-xml-document-into-the-dom"></a>将 XML 文档读入 DOM

XML 信息从不同的格式读入内存。 读取源包括字符串、流、URL、文本读取器或 <xref:System.Xml.XmlReader> 的派生类。  
  
 <xref:System.Xml.XmlDocument.Load%2A> 方法将文档置入内存中并包含可用于从每个不同的格式中获取数据的重载方法。 还存在 <xref:System.Xml.XmlDocument.LoadXml%2A> 方法，该方法从字符串中读取 XML。  
  
 不同的 <xref:System.Xml.XmlDocument.Load%2A> 方法影响在加载 XML 文档对象模型 (DOM) 时创建的节点。 下表列出了一些 <xref:System.Xml.XmlDocument.Load%2A> 方法的区别以及讲述这些区别的主题。  
  
|Subject|主题|  
|-------------|-----------|  
|创建空白节点|用来加载 DOM 的对象对 DOM 中生成的空白和有效空白节点有影响。 有关详细信息，请参阅[加载 DOM 时处理空格和有效空格](white-space-and-significant-white-space-handling-when-loading-the-dom.md)。|  
|从特定节点开始加载 XML 或加载整个 XML 文档|使用 <xref:System.Xml.XmlDocument.Load%2A?displayProperty=nameWithType> 方法，可以将数据从特定节点加载到 DOM 中。 有关详细信息，请参阅[从读取器加载数据](load-data-from-a-reader.md)。|  
|在 XML 加载时进行验证|加载到 DOM 中的 XML 数据可以在加载时进行验证。 使用验证 <xref:System.Xml.XmlReader> 可以完成此操作。 若要详细了解如何在加载 XML 时验证它，请参阅[在 DOM 中验证 XML 文档](validating-an-xml-document-in-the-dom.md)。|  
  
 以下示例显示使用 <xref:System.Xml.XmlDocument.LoadXml%2A> 方法加载的 XML 以及之后保存到称为 `data.xml` 的文本文件的数据。  
  
```vb  
Imports System  
Imports System.IO  
Imports System.Xml  
  
Public Class Sample  
  
    Public Shared Sub Main()  
        ' Create the XmlDocument.  
        Dim doc As New XmlDocument()  
        doc.LoadXml(("<book genre='novel' ISBN='1-861001-57-5'>" & _  
                    "<title>Pride And Prejudice</title>" & _  
                    "</book>"))  
        ' Save the document to a file.  
        doc.Save("data.xml")  
    End Sub 'Main  
End Class 'Sample  
```  
  
```csharp  
using System;  
using System.IO;  
using System.Xml;  
  
public class Sample  
{  
    public static void Main()  
    {  
        // Create the XmlDocument.  
        XmlDocument doc = new XmlDocument();  
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5'>" +  
                    "<title>Pride And Prejudice</title>" +  
                    "</book>");  
  
        // Save the document to a file.  
        doc.Save("data.xml");  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
