---
title: 如何：迁移 XslTransform 代码
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 910beb2f-cfb3-4e8e-9936-f7e0c5f4064a
ms.openlocfilehash: d1e13d2bcc7525f261df90c36aa4214f318cf3c0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722708"
---
# <a name="how-to-migrate-your-xsltransform-code"></a>如何：迁移 XslTransform 代码

业已设计的新 XSLT 类与现有类非常相似。 <xref:System.Xml.Xsl.XslCompiledTransform> 类替换 <xref:System.Xml.Xsl.XslTransform> 类。 样式表使用 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 方法进行编译。 使用 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法执行转换。 下面的过程演示常见 XSLT 任务，并比较使用 <xref:System.Xml.Xsl.XslTransform> 类和使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类的代码。  
  
### <a name="to-transform-a-file-and-output-to-a-uri"></a>转换文件和输出到 URI  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#9](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#9)]
     [!code-vb[XML_Migration#9](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#9)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
     [!code-csharp[XML_Migration#10](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#10)]
     [!code-vb[XML_Migration#10](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#10)]  
  
### <a name="to-compile-a-style-sheet-and-use-a-resolver-with-default-credentials"></a>编译样式表和使用具有默认凭据的解析器  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#11](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#11)]
     [!code-vb[XML_Migration#11](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#11)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
     [!code-csharp[XML_Migration#12](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#12)]
     [!code-vb[XML_Migration#12](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#12)]  
  
### <a name="to-use-an-xslt-parameter"></a>使用 XSLT 参数  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#13](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#13)]
     [!code-vb[XML_Migration#13](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#13)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
     [!code-csharp[XML_Migration#14](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#14)]
     [!code-vb[XML_Migration#14](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#14)]  
  
### <a name="to-enable-xslt-scripting"></a>启用 XSLT 脚本撰写  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#15](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#15)]
     [!code-vb[XML_Migration#15](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#15)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
     [!code-csharp[XML_Migration#16](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#16)]
     [!code-vb[XML_Migration#16](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#16)]  
  
### <a name="to-load-the-results-into-a-dom-object"></a>将结果加载到 DOM 对象  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#19](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#19)]
     [!code-vb[XML_Migration#19](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#19)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
    > [!NOTE]
    > <xref:System.Xml.Xsl.XslCompiledTransform> 类没有以 <xref:System.Xml.XmlReader> 对象形式返回 XSLT 转换结果的方法。 不过，您可以将结果输出到 XML 文件并将该 XML 文件加载到其他对象。  
  
     [!code-csharp[XML_Migration#20](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#20)]
     [!code-vb[XML_Migration#20](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#20)]  
  
### <a name="to-stream-the-results-into-another-data-store"></a>将结果流处理到其他数据存储区  
  
- 使用 <xref:System.Xml.Xsl.XslTransform> 类编码。  
  
     [!code-csharp[XML_Migration#17](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#17)]
     [!code-vb[XML_Migration#17](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#17)]  
  
- 使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类编码。  
  
     [!code-csharp[XML_Migration#18](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#18)]
     [!code-vb[XML_Migration#18](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#18)]  
  
## <a name="see-also"></a>请参阅

- [从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)
- [使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)
