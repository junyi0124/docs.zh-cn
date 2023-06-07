---
title: LINQ to XML 概述
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ to XML [Visual Basic], about LINQ to XML
- LINQ [Visual Basic], LINQ to XML
ms.assetid: 01c62a79-6d58-468e-84fb-039c05947701
ms.openlocfilehash: 4ec1c96bdca96a6e9b68b240c147b70536514d85
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91099185"
---
# <a name="overview-of-linq-to-xml-in-visual-basic"></a>Visual Basic 中的 LINQ to XML 概述

Visual Basic [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 通过 xml 文本和 xml 轴属性提供对的支持。 这使你可以使用熟悉的方便语法在 Visual Basic 代码中处理 XML。 使用*xml 文本*可以将 xml 直接包含在代码中。 利用*xml 轴属性*，您可以访问 xml 文本的子节点、子代节点和属性。 有关详细信息，请参阅 [Xml 文本概述](xml-literals-overview.md) 和 [访问 VISUAL BASIC 中的 xml](accessing-xml.md)。  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 是内存中的 XML 编程 API，旨在利用 (LINQ) 的语言集成查询。 尽管可以直接调用 LINQ Api，但只有 Visual Basic 允许你声明 XML 文本和直接访问 XML 轴属性。  
  
> [!NOTE]
> ASP.NET 页面中的声明性代码不支持 XML 文本和 XML 轴属性。 若要使用 Visual Basic XML 功能，请将代码放在 ASP.NET 应用程序的代码隐藏页中。  
  
 [播放按钮](./media/overview-of-linq-to-xml/play-video-icon-example.gif) 有关相关的视频演示，请参阅 [如何开始使用 LINQ to XML？](/aspnet/web-forms/videos/data-access/linq-videos-from-the-vb-team/how-do-i-get-started-with-linq-to-xml) 和 [如何使用 LINQ to XML 创建 Excel 电子表格？](/aspnet/web-forms/videos/data-access/linq-videos-from-the-vb-team/how-do-i-create-excel-spreadsheets-using-linq-to-xml)。
  
## <a name="creating-xml"></a>创建 XML  

 可以通过两种方式在 Visual Basic 中创建 XML 树。 你可以直接在代码中声明 XML 文本，或者可以使用 LINQ Api 创建树。 这两个进程都使代码可以反映 XML 树的最终结构。 例如，下面的代码示例创建一个 XML 元素：  
  
 [!code-vb[VbXmlSamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#5)]  
  
 有关详细信息，请参阅 [在 Visual Basic 中创建 XML](creating-xml.md)。  
  
## <a name="accessing-and-navigating-xml"></a>访问和导航 XML  

 Visual Basic 提供了用于访问和导航 XML 结构的 XML 轴属性。 这些属性使你可以通过指定 XML 子元素名称来访问 XML 元素和特性。 或者，你可以显式调用用于导航和查找元素和属性的 LINQ 方法。 例如，下面的代码示例使用 XML 轴属性来引用 XML 元素的特性和子元素。 此代码示例使用 LINQ 查询来检索子元素，并将其输出为 XML 元素，从而有效地执行转换。  
  
 [!code-vb[VbXmlSamples#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples3.vb#8)]  
  
 有关详细信息，请参阅 [Visual Basic 中的访问 XML](accessing-xml.md)。  
  
## <a name="xml-namespaces"></a>XML 命名空间  

 Visual Basic 使你能够使用语句指定全局 XML 命名空间的别名 `Imports` 。 下面的示例演示如何使用 `Imports` 语句导入 XML 命名空间：  
  
 [!code-vb[VbXMLSamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#1)]  
  
 访问 XML 轴属性并声明 xml 文档和元素的 XML 文本时，可以使用 XML 命名空间别名。  
  
 可以 <xref:System.Xml.Linq.XNamespace> 使用 [GetXmlNamespace 运算符](../../../language-reference/operators/getxmlnamespace-operator.md)检索特定命名空间前缀的对象。  
  
 有关详细信息，请参阅 [ (XML 命名空间) 的 Imports 语句 ](../../../language-reference/statements/imports-statement-xml-namespace.md)。  
  
### <a name="using-xml-namespaces-in-xml-literals"></a>在 XML 文本中使用 XML 命名空间  

 下面的示例演示如何创建 <xref:System.Xml.Linq.XElement> 使用 global 命名空间的对象 `ns` ：  
  
 [!code-vb[VbXMLSamples#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#2)]  
  
 Visual Basic 编译器将包含 XML 命名空间别名的 XML 文本转换为等效代码，该代码使用 XML 表示法，使用 xml 命名空间和 `xmlns` 属性。 在编译时，上一部分的示例中的代码将产生实质上与以下示例相同的可执行代码：  
  
 [!code-vb[VbXMLSamples#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#3)]  
  
### <a name="using-xml-namespaces-in-xml-axis-properties"></a>在 XML 轴属性中使用 XML 命名空间  

 在 xml 文本中声明的 XML 命名空间不可用于 XML 轴属性。 但是，可以将全局命名空间与 XML 轴属性一起使用。 使用冒号分隔 XML 命名空间前缀和本地元素名称。 下面是一个示例：  
  
 [!code-vb[VbXMLSamples#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples1.vb#4)]  
  
## <a name="see-also"></a>请参阅

- [XML](index.md)
- [在 Visual Basic 中创建 XML](creating-xml.md)
- [在 Visual Basic 中访问 XML](accessing-xml.md)
- [在 Visual Basic 中操作 XML](manipulating-xml.md)
