---
title: 使用 XPathNavigator 访问强类型 XML 数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 898e0f52-8a7c-4d1f-afcd-6ffb28b050b4
ms.openlocfilehash: 7051aeb8cdc25518f99fe093045e7e769ae7f6f5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725412"
---
# <a name="accessing-strongly-typed-xml-data-using-xpathnavigator"></a>使用 XPathNavigator 访问强类型 XML 数据

作为 XPath 2.0 数据模型的实例，<xref:System.Xml.XPath.XPathNavigator> 类可以包含映射到公共语言运行库 (CLR) 类型的强类型数据。 根据 XPath 2.0 数据模型，只有元素和属性可以包含强类型数据。 <xref:System.Xml.XPath.XPathNavigator> 类提供将 <xref:System.Xml.XPath.XPathDocument> 或 <xref:System.Xml.XmlDocument> 对象中的数据作为强类型数据访问的机制，以及将一种数据类型转换为另一种数据类型的机制。  
  
## <a name="type-information-exposed-by-xpathnavigator"></a>通过 XPathNavigator 公开的类型信息  

 XML 1.0 数据在技术角度没有类型，除非使用 DTD、XML 架构定义语言 (XSD) 架构或其他机制进行处理。 有许多类别的类型信息可以与 XML 元素或属性关联。  
  
- 简单 CLR 类型：所有 XML 架构语言都不直接支持公共语言运行时 (CLR) 类型。 因为能够以最适合的 CLR 类型查看简单元素和属性内容非常有用，所以，在缺少架构信息以及任何添加的架构信息（可能会将此内容优化为更适合的类型）时，可以将所有简单内容类型化为 <xref:System.String>。 可以使用 <xref:System.Xml.XPath.XPathNavigator.ValueType%2A> 属性找到简单元素和属性内容最匹配的 CLR 类型。 若要详细了解如何从架构内置类型映射到 CLR 类型，请参阅 [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)。  
  
- 简单 (CLR) 类型列表：包含简单内容的元素或属性可以包含以空格分隔的值列表。 XML 架构将这些值指定为“列表类型”。 在缺少 XML 架构时，此类简单内容将作为单个文本节点对待。 在 XML 架构可用时，此简单内容可以作为一系列原子值公开，每个值都具有一种映射到 CLR 对象集合的简单类型。 若要详细了解如何从架构内置类型映射到 CLR 类型，请参阅 [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)。  
  
- 类型化值：包含简单内容的已经过架构验证的属性或元素有类型化值。 此值是基元类型，例如数字、字符串或日期类型。 XSD 中的所有内置简单类型均可以映射到 CLR 类型，通过 CLR 类型可以以更适合的类型（而不只是以 <xref:System.String>）访问节点的值。 具有属性或元素子级的元素被认为是复杂类型。 包含简单内容（只有文本节点作为子级）的复杂类型的类型化值与其内容的简单类型的类型化值相同。 包含复杂内容（一个或多个子元素）的复杂类型的类型化值是串联以 <xref:System.String> 形式返回的所有子文本节点的字符串值。 若要详细了解如何从架构内置类型映射到 CLR 类型，请参阅 [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)。  
  
- 架构语言专用类型名称：在大多数情况下，作为应用外部架构附带后果设置的 CLR 类型用于提供对节点值的访问权限。 但是，有时可能需要检查与应用于 XML 文档的特定架构相关联的类型。 例如，可能希望搜索 XML 文档，根据附加的架构提取确定包含“PurchaseOrder”类型内容的所有元素。 此类信息只能设置为架构验证的结果，此信息通过 <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> 和 <xref:System.Xml.XPath.XPathNavigator> 属性访问。 有关更多信息，请参见下面的“后架构验证信息集 (PSVI)”一节。  
  
- 架构语言专用类型反射：在其他情况下，建议获取应用于 XML 文档的架构专用类型的更多详细信息。 例如，在读取 XML 文件时，可能需要为 XML 文档中的每个有效节点提取 `maxOccurs` 属性，以便执行某项自定义计算。 因为此信息仅通过架构验证设置，所以，通过 <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 属性访问。 有关更多信息，请参见下面的“后架构验证信息集 (PSVI)”一节。  
  
## <a name="xpathnavigator-typed-accessors"></a>XPathNavigator 类型化访问器  

 下表显示 <xref:System.Xml.XPath.XPathNavigator> 类中可以用于访问节点的类型信息的各种属性和方法。  
  
|Property|描述|  
|--------------|-----------------|  
|<xref:System.Xml.XPath.XPathNavigator.XmlType%2A>|此属性包含节点（如果有效）的 XML 架构类型信息。|  
|<xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A>|此属性包含在验证之后添加的节点的后架构验证信息集。 其中包括 XML 架构类型信息以及有效性信息。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueType%2A>|节点的类型化值的 CLR 类型。|  
|<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A>|作为类型与节点的 XML 架构类型最匹配的一个或多个 CLR 值的节点内容。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsBoolean%2A>|当前节点的 <xref:System.String> 值根据 <xref:System.Boolean> 的 XPath 2.0 强制转换规则强制转换为 `xs:boolean` 值。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDateTime%2A>|当前节点的 <xref:System.String> 值根据 <xref:System.DateTime> 的 XPath 2.0 强制转换规则强制转换为 `xs:datetime` 值。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>|当前节点的 <xref:System.String> 值根据 <xref:System.Double> 的 XPath 2.0 强制转换规则强制转换为 `xsd:double` 值。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A>|当前节点的 <xref:System.String> 值根据 <xref:System.Int32> 的 XPath 2.0 强制转换规则强制转换为 `xs:integer` 值。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A>|当前节点的 <xref:System.String> 值根据 <xref:System.Int64> 的 XPath 2.0 强制转换规则强制转换为 `xs:integer` 值。|  
|<xref:System.Xml.XPath.XPathNavigator.ValueAs%2A>|节点内容根据 XPath 2.0 强制转换规则强制转换为目标类型。|  
  
 若要详细了解如何从架构内置类型映射到 CLR 类型，请参阅 [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)。  
  
## <a name="the-post-schema-validation-infoset-psvi"></a>后架构验证信息集 (PSVI)  

 XML 架构处理器使用 XML 信息集作为输入，并将其转换为后架构验证信息集 (PSVI)。 PSVI 是原始输入 XML 信息集，包含添加的新信息项以及在现有信息项中添加的新属性。 在 PSVI 的 XML 信息集中添加了三种广义信息类，通过 <xref:System.Xml.XPath.XPathNavigator> 公开。  
  
1. 验证结果：有关是否已成功验证元素或属性的信息。 此信息通过 <xref:System.Xml.Schema.IXmlSchemaInfo.Validity%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> 属性的 <xref:System.Xml.XPath.XPathNavigator> 属性公开。  
  
2. 默认信息：有关是否已通过架构中指定的默认值获取元素或属性的值的信息。 此信息通过 <xref:System.Xml.Schema.IXmlSchemaInfo.IsDefault%2A> 类的 <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> 属性的 <xref:System.Xml.XPath.XPathNavigator> 属性公开。  
  
3. 类型批注：对架构组件的引用，可能是类型定义或元素和属性的声明。 <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> 的 <xref:System.Xml.XPath.XPathNavigator> 属性包含节点（如果有效）的特定类型信息。 如果节点的有效性未知，例如节点在验证后进行了编辑， <xref:System.Xml.XPath.XPathNavigator.XmlType%2A> 属性将设置为 `null`，但是类型信息仍可以通过 <xref:System.Xml.XPath.XPathNavigator.SchemaInfo%2A> 类的 <xref:System.Xml.XPath.XPathNavigator> 属性的各种属性访问。  
  
 以下示例说明如何使用通过 <xref:System.Xml.XPath.XPathNavigator> 公开的后架构验证信息集中的信息。  
  
```vb  
Dim settings As XmlReaderSettings = New XmlReaderSettings()  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd")  
settings.ValidationType = ValidationType.Schema  
  
Dim reader As XmlReader = XmlReader.Create("books.xml", settings)  
  
Dim document As XmlDocument = New XmlDocument()  
document.Load(reader)  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "http://www.contoso.com/books")  
navigator.MoveToChild("book", "http://www.contoso.com/books")  
navigator.MoveToChild("published", "http://www.contoso.com/books")  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name)  
Console.WriteLine(navigator.SchemaInfo.Validity)  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs)  
```  
  
```csharp  
XmlReaderSettings settings = new XmlReaderSettings();  
settings.Schemas.Add("http://www.contoso.com/books", "books.xsd");  
settings.ValidationType = ValidationType.Schema;  
  
XmlReader reader = XmlReader.Create("books.xml", settings);  
  
XmlDocument document = new XmlDocument();  
document.Load(reader);  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "http://www.contoso.com/books");  
navigator.MoveToChild("book", "http://www.contoso.com/books");  
navigator.MoveToChild("published", "http://www.contoso.com/books");  
  
Console.WriteLine(navigator.SchemaInfo.SchemaType.Name);  
Console.WriteLine(navigator.SchemaInfo.Validity);  
Console.WriteLine(navigator.SchemaInfo.SchemaElement.MinOccurs);  
```  
  
 该示例使用 `books.xml` 文件作为输入。  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 该示例还使用 `books.xsd` 架构作为输入。  
  
```xml  
<xs:schema xmlns="http://www.contoso.com/books"
attributeFormDefault="unqualified" elementFormDefault="qualified"
targetNamespace="http://www.contoso.com/books"
xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:simpleType name="publishedType">  
        <xs:restriction base="xs:date">  
            <xs:minInclusive value="2003-01-01" />  
            <xs:maxInclusive value="2003-12-31" />  
        </xs:restriction>  
    </xs:simpleType>  
    <xs:complexType name="bookType">  
        <xs:sequence>  
            <xs:element name="title" type="xs:string"/>  
            <xs:element name="price" type="xs:decimal"/>  
            <xs:element name="published" type="publishedType"/>  
        </xs:sequence>  
    </xs:complexType>  
    <xs:complexType name="booksType">  
        <xs:sequence>  
            <xs:element name="book" type="bookType" />  
        </xs:sequence>  
    </xs:complexType>  
    <xs:element name="books" type="booksType" />  
</xs:schema>  
```  
  
## <a name="obtain-typed-values-using-valueas-properties"></a>使用 ValueAs 属性获取类型化值  

 节点的类型化值可以通过访问 <xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> 的 <xref:System.Xml.XPath.XPathNavigator> 属性进行检索。 在某些情况下，可能需要将节点的类型化值转换为其他类型。 常见的示例是从 XML 节点获取数值。 例如，考虑以下未经过验证和非类型化的 XML 文档。  
  
```xml  
<books>  
    <book>  
        <title>Title</title>  
        <price>10.00</price>  
        <published>2003-12-31</published>  
    </book>  
</books>  
```  
  
 如果 <xref:System.Xml.XPath.XPathNavigator> 位于 `price` 元素上，<xref:System.Xml.XPath.XPathNavigator.XmlType%2A> 属性将为 `null`，<xref:System.Xml.XPath.XPathNavigator.ValueType%2A> 属性将为 <xref:System.String>，<xref:System.Xml.XPath.XPathNavigator.TypedValue%2A> 属性将为字符串 `10.00`。  
  
 但是，仍可以使用 <xref:System.Xml.XPath.XPathItem.ValueAs%2A>、<xref:System.Xml.XPath.XPathNavigator.ValueAsDouble%2A>、<xref:System.Xml.XPath.XPathNavigator.ValueAsInt%2A> 或 <xref:System.Xml.XPath.XPathNavigator.ValueAsLong%2A> 方法和属性以数值形式提取该值。 以下示例说明如何使用 <xref:System.Xml.XPath.XPathItem.ValueAs%2A> 方法执行此类强制转换。  
  
```vb  
Dim document As New XmlDocument()  
document.Load("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
navigator.MoveToChild("books", "")  
navigator.MoveToChild("book", "")  
navigator.MoveToChild("price", "")  
  
Dim price = navigator.ValueAs(GetType(Decimal))  
Dim discount As Decimal = 0.2  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * discount))  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
navigator.MoveToChild("books", "");  
navigator.MoveToChild("book", "");  
navigator.MoveToChild("price", "");  
  
Decimal price = (decimal)navigator.ValueAs(typeof(decimal));  
  
Console.WriteLine("The price of the book has been dropped 20% from {0:C} to {1:C}", navigator.Value, (price - price * (decimal)0.20));  
```  
  
 若要详细了解如何从架构内置类型映射到 CLR 类型，请参阅 [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [System.Xml 类中的类型支持](type-support-in-the-system-xml-classes.md)
- [使用 XPath 数据模型处理 XML 数据](process-xml-data-using-the-xpath-data-model.md)
- [使用 XPathNavigator 的节点集定位](node-set-navigation-using-xpathnavigator.md)
- [使用 XPathNavigator 的属性和命名空间节点定位](attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [使用 XPathNavigator 提取 XML 数据](extract-xml-data-using-xpathnavigator.md)
