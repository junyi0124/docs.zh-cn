---
title: 使用 XPathNavigator 验证架构
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 81fa0e41-d9c9-46f0-b22b-50da839c77f5
ms.openlocfilehash: 1c91dfa63723cc087662630232376e74394c7b13
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734694"
---
# <a name="schema-validation-using-xpathnavigator"></a>使用 XPathNavigator 验证架构

使用 <xref:System.Xml.XmlDocument> 类，可以通过两种方式验证 <xref:System.Xml.XmlDocument> 对象中包含的 XML 内容。 第一种方式是使用验证 <xref:System.Xml.XmlReader> 对象验证 XML 内容，第二种方式是使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法。 还可以使用 <xref:System.Xml.XPath.XPathDocument> 类对 XML 内容执行只读验证。  
  
## <a name="validating-xml-data"></a>验证 XML 数据  

 默认情况下，<xref:System.Xml.XmlDocument> 类不使用 DTD 或 XML 架构定义语言 (XSD) 架构验证来验证 XML 文档。 只验证 XML 文档的格式是否正确。  
  
 第一种验证 XML 文档的方式是使用验证 <xref:System.Xml.XmlDocument> 对象，在文档加载到 <xref:System.Xml.XmlReader> 对象时进行验证。 第二种方式是使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法验证以前非类型化的 XML 文档。 在这两种情况下，均可以使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法重新验证对经过验证的 XML 文档的更改。  
  
### <a name="validating-a-document-as-it-is-loaded"></a>在文档加载时进行验证  

 验证 <xref:System.Xml.XmlReader> 对象通过将 <xref:System.Xml.XmlReaderSettings> 对象传递给 <xref:System.Xml.XmlReader.Create%2A> 类的 <xref:System.Xml.XmlReader> 方法进行创建，该方法使用 <xref:System.Xml.XmlReaderSettings> 对象作为参数。 作为参数传递的 <xref:System.Xml.XmlReaderSettings> 对象将 <xref:System.Xml.XmlReaderSettings.ValidationType%2A> 属性设置为 `Schema`，并将 <xref:System.Xml.XmlDocument> 对象中包含的 XML 文档的 XML 架构添加到其 <xref:System.Xml.XmlReaderSettings.Schemas%2A> 属性中。 然后，使用验证 <xref:System.Xml.XmlReader> 对象创建 <xref:System.Xml.XmlDocument> 对象。  
  
 以下示例在 `contosoBooks.xml` 文件加载到 <xref:System.Xml.XmlDocument> 对象时进行验证，方法是使用验证 <xref:System.Xml.XmlDocument> 对象创建 <xref:System.Xml.XmlReader> 对象。 因为 XML 文档符合其架构，所以，未生成任何架构验证错误或警告。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 该示例还使用 `contosoBooks.xsd` 作为输入。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 在上面的示例中，如果任何属性或元素类型不匹配验证架构中指定的相应类型，则调用 <xref:System.Xml.Schema.XmlSchemaValidationException> 时将引发 <xref:System.Xml.XmlDocument.Load%2A>。 如果在验证 <xref:System.Xml.XmlReaderSettings.ValidationEventHandler> 时设置 <xref:System.Xml.XmlReader>，则遇到无效类型时将调用 <xref:System.Xml.XmlReaderSettings.ValidationEventHandler>。  
  
 由 <xref:System.Xml.Schema.XmlSchemaException> 访问 <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> 设置为 `invalid` 的属性或元素时，将引发 <xref:System.Xml.XPath.XPathNavigator>。  
  
 <xref:System.Xml.Schema.XmlSchemaInfo.Validity%2A> 属性可用于确定用 <xref:System.Xml.XPath.XPathNavigator> 访问属性或元素时单个属性或元素是否有效。  
  
> [!NOTE]
> 如果 XML 文档加载到 <xref:System.Xml.XmlDocument> 对象，并且具有关联的架构来定义默认值，<xref:System.Xml.XmlDocument> 对象对待这些默认值就像出现在 XML 文档中一样。 这意味着对于架构中的默认元素，<xref:System.Xml.XPath.XPathNavigator.IsEmptyElement%2A> 属性始终返回 `false`，即使在 XML 文档中，也将编写为空元素。  
  
### <a name="validating-a-document-using-the-validate-method"></a>使用验证方法验证文档  

 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法针对 <xref:System.Xml.XmlDocument> 对象的 <xref:System.Xml.XmlDocument> 属性中指定的架构验证 <xref:System.Xml.XmlDocument.Schemas%2A> 对象中包含的 XML 文档，然后执行信息集增加。 结果是 <xref:System.Xml.XmlDocument> 中以前非类型化的 XML 文档由类型化的文档取代。  
  
 <xref:System.Xml.XmlDocument> 对象使用作为参数传递给 <xref:System.Xml.Schema.ValidationEventHandler> 方法的 <xref:System.Xml.XmlDocument.Validate%2A> 委托报告架构验证错误和警告。  
  
 以下示例针对 `contosoBooks.xml` 对象的 <xref:System.Xml.XmlDocument> 属性中包含的 `contosoBooks.xsd` 架构验证 <xref:System.Xml.XmlDocument> 对象中包含的 <xref:System.Xml.XmlDocument.Schemas%2A> 文件。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidateExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Dim document As XmlDocument = New XmlDocument()  
        document.Load("contosoBooks.xml")  
        Dim navigator As XPathNavigator = document.CreateNavigator()  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
        Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
        document.Validate(validation)  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidateExample  
{  
    static void Main(string[] args)  
    {  
        XmlDocument document = new XmlDocument();  
        document.Load("contosoBooks.xml");  
        XPathNavigator navigator = document.CreateNavigator();  
  
        document.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
        ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
        document.Validate(validation);  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 该示例还使用 `contosoBooks.xsd` 作为输入。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
### <a name="validating-modifications"></a>验证修改  

 对 XML 文档进行修改之后，可以使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法针对 XML 文档的架构验证修改。  
  
 以下示例在 `contosoBooks.xml` 文件加载到 <xref:System.Xml.XmlDocument> 对象时进行验证，方法是使用验证 <xref:System.Xml.XmlDocument> 对象创建 <xref:System.Xml.XmlReader> 对象。 XML 文档在加载时成功进行验证，未生成任何架构验证错误或警告。 然后，示例对 XML 文档进行了两处符合 `contosoBooks.xsd` 架构的修改。 第一处修改是插入一个无效的子元素，从而引发架构验证错误，第二处修改是将类型化节点的值设置为不符合节点类型的值，从而引发异常。  
  
```vb  
Imports System  
Imports System.Xml  
Imports System.Xml.Schema  
Imports System.Xml.XPath  
  
Class ValidatingReaderExample  
  
    Shared Sub Main(ByVal args() As String)  
  
        Try  
            Dim settings As XmlReaderSettings = New XmlReaderSettings()  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd")  
            settings.ValidationType = ValidationType.Schema  
  
            Dim reader As XmlReader = XmlReader.Create("contosoBooks.xml", settings)  
  
            Dim document As XmlDocument = New XmlDocument()  
            document.Load(reader)  
            Dim navigator As XPathNavigator = document.CreateNavigator()  
  
            Dim validation As ValidationEventHandler = New ValidationEventHandler(AddressOf SchemaValidationHandler)  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books")  
            navigator.MoveToChild("book", "http://www.contoso.com/books")  
            navigator.MoveToChild("author", "http://www.contoso.com/books")  
  
            navigator.AppendChild("<title>Book Title</title>")  
  
            document.Validate(validation)  
  
            navigator.MoveToParent()  
            navigator.MoveToChild("price", "http://www.contoso.com/books")  
            navigator.SetTypedValue(DateTime.Now)  
        Catch e As Exception  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message)  
        End Try  
  
    End Sub  
  
    Shared Sub SchemaValidationHandler(ByVal sender As Object, ByVal e As ValidationEventArgs)  
  
        Select Case e.Severity  
            Case XmlSeverityType.Error  
                Console.WriteLine("Schema Validation Error: {0}", e.Message)  
                Exit Sub  
            Case XmlSeverityType.Warning  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message)  
                Exit Sub  
        End Select  
  
    End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Xml;  
using System.Xml.Schema;  
using System.Xml.XPath;  
  
class ValidatingReaderExample  
{  
    static void Main(string[] args)  
    {  
        try  
        {  
            XmlReaderSettings settings = new XmlReaderSettings();  
            settings.Schemas.Add("http://www.contoso.com/books", "contosoBooks.xsd");  
            settings.ValidationType = ValidationType.Schema;  
  
            XmlReader reader = XmlReader.Create("contosoBooks.xml", settings);  
  
            XmlDocument document = new XmlDocument();  
            document.Load(reader);  
            XPathNavigator navigator = document.CreateNavigator();  
  
            ValidationEventHandler validation = new ValidationEventHandler(SchemaValidationHandler);  
  
            navigator.MoveToChild("bookstore", "http://www.contoso.com/books");  
            navigator.MoveToChild("book", "http://www.contoso.com/books");  
            navigator.MoveToChild("author", "http://www.contoso.com/books");  
  
            navigator.AppendChild("<title>Book Title</title>");  
  
            document.Validate(validation);  
  
            navigator.MoveToParent();  
            navigator.MoveToChild("price", "http://www.contoso.com/books");  
            navigator.SetTypedValue(DateTime.Now);  
        }  
        catch (Exception e)  
        {  
            Console.WriteLine("ValidatingReaderExample.Exception: {0}", e.Message);  
        }  
    }  
  
    static void SchemaValidationHandler(object sender, ValidationEventArgs e)  
    {  
        switch (e.Severity)  
        {  
            case XmlSeverityType.Error:  
                Console.WriteLine("Schema Validation Error: {0}", e.Message);  
                break;  
            case XmlSeverityType.Warning:  
                Console.WriteLine("Schema Validation Warning: {0}", e.Message);  
                break;  
        }  
    }  
}  
```  
  
 该示例使用 `contosoBooks.xml` 文件作为输入。  
  
 [!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]  
  
 该示例还使用 `contosoBooks.xsd` 作为输入。  
  
 [!code-xml[XPathXMLExamples#3](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xsd#3)]  
  
 在上面的示例中，对 <xref:System.Xml.XmlDocument> 对象中包含的 XML 文档进行了两处修改。 在 XML 文档加载时，遇到的任何架构验证错误将通过验证事件处理程序方法进行处理并写入控制台。  
  
 在此示例中，加载 XML 文档之后引入验证错误，并使用 <xref:System.Xml.XmlDocument.Validate%2A> 类的 <xref:System.Xml.XmlDocument> 方法发现错误。  
  
 使用 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 方法进行的修改引发了 <xref:System.InvalidCastException>，因为新值不符合节点的架构类型。  
  
 若要详细了解如何使用 <xref:System.Xml.XPath.XPathNavigator.SetTypedValue%2A> 方法修改值，请参阅[使用 XPathNavigator 修改 XML 数据](modify-xml-data-using-xpathnavigator.md)主题。  
  
### <a name="read-only-validation"></a>只读验证  

 <xref:System.Xml.XPath.XPathDocument> 类是 XML 文档在内存中的只读表示形式。 <xref:System.Xml.XPath.XPathDocument> 类和 <xref:System.Xml.XmlDocument> 类均创建 <xref:System.Xml.XPath.XPathNavigator> 对象来浏览和编辑 XML 文档。 因为 <xref:System.Xml.XPath.XPathDocument> 类是只读类，从 <xref:System.Xml.XPath.XPathNavigator> 对象返回的 <xref:System.Xml.XPath.XPathDocument> 对象不能编辑 <xref:System.Xml.XPath.XPathDocument> 对象中包含的 XML 文档。  
  
 在验证时，可以创建一个 <xref:System.Xml.XPath.XPathDocument> 对象，就好像使用验证 <xref:System.Xml.XmlDocument> 对象创建 <xref:System.Xml.XmlReader> 对象一样，如本主题前面所述。 <xref:System.Xml.XPath.XPathDocument> 对象在 XML 文档加载时进行验证，但是因为不能编辑 <xref:System.Xml.XPath.XPathDocument> 对象中的 XML 数据，所以，无法重新验证 XML 文档。  
  
 若要详细了解只读和可编辑 <xref:System.Xml.XPath.XPathNavigator> 对象，请参阅[使用 XPathDocument 和 XmlDocument 读取 XML 数据](reading-xml-data-using-xpathdocument-and-xmldocument.md)主题。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathDocument 和 XmlDocument 读取 XML 数据](reading-xml-data-using-xpathdocument-and-xmldocument.md)
- [使用 XPathNavigator 选择、计算和匹配 XML 数据](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 访问 XML 数据](accessing-xml-data-using-xpathnavigator.md)
- [使用 XPathNavigator 编辑 XML 数据](editing-xml-data-using-xpathnavigator.md)
