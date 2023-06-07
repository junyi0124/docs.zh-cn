---
title: 在 DOM 中验证 XML 文档
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 2c61c920-d0f8-4c72-bfcc-6524570f3060
ms.openlocfilehash: db3748d1d0e1a5687219d5b07d1261639a4ef670
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819287"
---
# <a name="validating-an-xml-document-in-the-dom"></a>在 DOM 中验证 XML 文档

默认情况下，在文档对象模型 (DOM) 中，<xref:System.Xml.XmlDocument> 类不针对 XML 架构定义语言 (XSD) 架构或文档类型定义 (DTD) 验证 XML；只验证 XML 的格式是否正确。

要在 DOM 中验证 XML，可以在 XML 加载到 DOM 时进行验证，方法是将架构验证 <xref:System.Xml.XmlReader> 传递给 <xref:System.Xml.XmlDocument.Load%2A> 类的 <xref:System.Xml.XmlDocument> 方法，也可以在 DOM 中使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法来验证以前未经过验证的 XML 文档。

## <a name="validating-an-xml-document-as-it-is-loaded-into-the-dom"></a>在 XML 文档加载到 DOM 时进行验证

如果将验证 <xref:System.Xml.XmlDocument> 传递给 <xref:System.Xml.XmlReader> 类的 <xref:System.Xml.XmlDocument.Load%2A> 方法，<xref:System.Xml.XmlDocument> 类将在 XML 数据加载到 DOM 时对其进行验证。

成功验证之后，将应用架构的默认值，文本值根据需要转换为原子值，类型信息与经过验证的信息项关联。 因此，类型化 XML 数据将替换以前非类型化的 XML 数据。

### <a name="creating-an-xml-schema-validating-xmlreader"></a>创建 XML 架构验证 XmlReader

要创建 XML 架构验证 <xref:System.Xml.XmlReader>，请执行下列步骤。

1. 构造一个新的 <xref:System.Xml.XmlReaderSettings> 实例。

2. 将 XML 架构添加到 <xref:System.Xml.XmlReaderSettings.Schemas%2A> 实例的 <xref:System.Xml.XmlReaderSettings> 属性中。

3. 将 `Schema` 指定为 <xref:System.Xml.XmlReaderSettings.ValidationType%2A>。

4. 可以选择指定 <xref:System.Xml.XmlReaderSettings.ValidationFlags%2A> 和 <xref:System.Xml.XmlReaderSettings.ValidationEventHandler>，用于处理在验证期间遇到的架构验证错误和警告。

5. 最后，将 <xref:System.Xml.XmlReaderSettings> 对象与 XML 文档一起传递给 <xref:System.Xml.XmlReader.Create%2A> 类的 <xref:System.Xml.XmlReader> 方法，创建架构验证 <xref:System.Xml.XmlReader>。

### <a name="example"></a>示例

在下面的代码示例中，架构验证 <xref:System.Xml.XmlReader> 验证加载到 DOM 的 XML 数据。 对 XML 文档进行无效的修改，然后重新验证文档，造成架构验证错误。 最后，更正其中一个错误，然后对 XML 文档的一部分进行部分验证。

[!code-cpp[XmlDocumentValidation.Load#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlDocumentValidation.Load/CPP/XmlDocumentValidationExample.cpp#1)]
[!code-csharp[XmlDocumentValidation.Load#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Load/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Load#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Load/VB/XmlDocumentValidationExample.vb#1)]

该示例使用 `contosoBooks.xml` 文件作为输入。

[!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]

该示例还使用 `contosoBooks.xsd` 文件作为输入。

[!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]

如果在 XML 数据加载到 DOM 时进行验证，请考虑下列事项。

- 在上述示例中，每当遇到无效类型时就会调用 <xref:System.Xml.XmlReaderSettings.ValidationEventHandler>。 如果 <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> 在验证 <xref:System.Xml.XmlReader> 时未设置，则在任何属性或元素类型与验证架构中指定的相应类型不匹配的情况下调用 <xref:System.Xml.Schema.XmlSchemaValidationException> 时会引发 <xref:System.Xml.XmlDocument.Load%2A>。

- 如果 XML 文档加载到 <xref:System.Xml.XmlDocument> 对象，并且具有关联的架构来定义默认值，<xref:System.Xml.XmlDocument> 会像在 XML 文档中一样对待这些默认值。 这意味着，对于架构中的默认元素，<xref:System.Xml.XmlReader.IsEmptyElement%2A> 属性始终返回 `false`。 即使在 XML 文档中，也将编写为空元素。

## <a name="validating-an-xml-document-in-the-dom"></a>在 DOM 中验证 XML 文档

<xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法针对 <xref:System.Xml.XmlDocument> 对象的 <xref:System.Xml.XmlDocument.Schemas%2A> 属性中的架构，验证加载到 DOM 的 XML 数据。 成功验证之后，将应用架构的默认值，文本值根据需要转换为原子值，类型信息与经过验证的信息项关联。 因此，类型化 XML 数据将替换以前非类型化的 XML 数据。

下面的示例与上面“在 XML 文档加载到 DOM 时进行验证”中的示例类似。 在此示例中，XML 文档不是在加载到 DOM 时进行验证，而是在加载到 DOM 之后，使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法进行验证。 <xref:System.Xml.XmlDocument.Validate%2A> 方法针对 <xref:System.Xml.XmlDocument.Schemas%2A> 的 <xref:System.Xml.XmlDocument> 属性中包含的 XML 架构验证 XML 文档。 对 XML 文档进行无效的修改，然后重新验证文档，造成架构验证错误。 最后，更正其中一个错误，然后对 XML 文档的一部分进行部分验证。

[!code-csharp[XmlDocumentValidation.Validate#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlDocumentValidation.Validate/CS/XmlDocumentValidationExample.cs#1)]
[!code-vb[XmlDocumentValidation.Validate#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlDocumentValidation.Validate/VB/XmlDocumentValidationExample.vb#1)]

该示例使用 `contosoBooks.xml` 和 `contosoBooks.xsd` 文件作为输入，这两个文件在上面的“在 XML 文档加载到 DOM 时进行验证”中提到。

## <a name="handling-validation-errors-and-warnings"></a>处理验证错误和警告

XML 架构验证错误在验证加载到 DOM 的 XML 数据时报告。 会通知您在以下过程中发现的所有架构验证错误：在验证正在加载的 XML 数据的时，或在验证以前未经过验证的 XML 文档时。

验证错误由 <xref:System.Xml.Schema.ValidationEventHandler> 进行处理。 如果 <xref:System.Xml.Schema.ValidationEventHandler> 已分配给 <xref:System.Xml.XmlReaderSettings> 实例或传递给 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法，<xref:System.Xml.Schema.ValidationEventHandler> 将处理架构验证错误；否则，在遇到架构验证错误时，将引发 <xref:System.Xml.Schema.XmlSchemaValidationException>。

> [!NOTE]
> 尽管遇到架构验证错误，XML 数据仍会加载到 DOM 中，<xref:System.Xml.Schema.ValidationEventHandler> 引发异常来停止该进程时除外。
>
> 除非为 <xref:System.Xml.Schema.XmlSchemaValidationFlags.ReportValidationWarnings> 对象指定了 <xref:System.Xml.XmlReaderSettings> 标志，否则，不会报告架构验证警告。

 有关说明 <xref:System.Xml.Schema.ValidationEventHandler> 的示例，请参见上面的“在 XML 文档加载到 DOM 时进行验证”和“在 DOM 中验证 XML 文档”。

## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XmlReader>
- <xref:System.Xml.Schema.ValidationEventHandler>
- <xref:System.Xml.XmlReaderSettings>
- [使用 DOM 模型处理 XML 数据](process-xml-data-using-the-dom-model.md)
- [使用 XML 架构](working-with-xml-schemas.md)
