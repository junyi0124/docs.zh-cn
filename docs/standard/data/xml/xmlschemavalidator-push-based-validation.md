---
title: XmlSchemaValidator 基于推送的验证
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 911d4460-dd91-4958-85b2-2ca3299f9ec6
ms.openlocfilehash: 9daf6f416d22cd06932cdaba1276889e319d3d90
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824891"
---
# <a name="xmlschemavalidator-push-based-validation"></a>XmlSchemaValidator 基于推送的验证

<xref:System.Xml.Schema.XmlSchemaValidator> 类提供了一种高效、高性能的机制，通过基于推送的方式针对 XML 架构验证 XML 数据。 例如，使用 <xref:System.Xml.Schema.XmlSchemaValidator> 类可以就地验证 XML 信息集，而不必将其序列化为 XML 文档，然后使用验证 XML 读取器重新分析该文档。

<xref:System.Xml.Schema.XmlSchemaValidator> 类可以在一些高级方案中使用（例如在自定义的 XML 数据源上生成验证引擎），也可以作为生成验证 XML 写入器的一种方式。

以下示例使用 <xref:System.Xml.Schema.XmlSchemaValidator> 类针对 `contosoBooks.xml` 架构验证 `contosoBooks.xsd` 文件。 该示例使用 <xref:System.Xml.Serialization.XmlSerializer> 类反序列化 `contosoBooks.xml` 文件，并将节点的值传递给 <xref:System.Xml.Schema.XmlSchemaValidator> 类的方法。

> [!NOTE]
> 此示例在本主题的所有章节中使用。

[!code-csharp[XmlSchemaValidatorExamples#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaValidatorExamples/CS/XmlSchemaValidatorExamples.cs#1)]
[!code-vb[XmlSchemaValidatorExamples#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaValidatorExamples/VB/XmlSchemaValidatorExamples.vb#1)]

该示例使用 `contosoBooks.xml` 文件作为输入。

[!code-xml[XPathXMLExamples#2](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/contosoBooks.xml#2)]

该示例还使用 `contosoBooks.xsd` 作为输入。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" targetNamespace="http://www.contoso.com/books" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="bookstore">
        <xs:complexType>
            <xs:sequence>
                <xs:element maxOccurs="unbounded" name="book">
                    <xs:complexType>
                        <xs:sequence>
                            <xs:element name="title" type="xs:string" />
                            <xs:element name="author">
                                <xs:complexType>
                                    <xs:sequence>
                                        <xs:element minOccurs="0" name="name" type="xs:string" />
                                        <xs:element minOccurs="0" name="first-name" type="xs:string" />
                                        <xs:element minOccurs="0" name="last-name" type="xs:string" />
                                    </xs:sequence>
                                </xs:complexType>
                            </xs:element>
                            <xs:element name="price" type="xs:decimal" />
                        </xs:sequence>
                        <xs:attribute name="genre" type="xs:string" use="required" />
                        <xs:attribute name="publicationdate" type="xs:date" use="required" />
                        <xs:attribute name="ISBN" type="xs:string" use="required" />
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="validating-xml-data-using-xmlschemavalidator"></a>使用 XmlSchemaValidator 验证 XML 数据

要开始验证 XML 信息集，必须先使用 <xref:System.Xml.Schema.XmlSchemaValidator> 构造函数初始化 <xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> 类的新实例。

<xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> 构造函数使用 <xref:System.Xml.XmlNameTable>、<xref:System.Xml.Schema.XmlSchemaSet> 和 <xref:System.Xml.XmlNamespaceManager> 对象作为参数，同时也将 <xref:System.Xml.Schema.XmlSchemaValidationFlags> 值用作参数。 <xref:System.Xml.XmlNameTable> 对象用于原子化众所周知的命名空间字符串，例如架构命名空间、XML 命名空间等，并在验证简单内容时传递给 <xref:System.Xml.Schema.XmlSchemaDatatype.ParseValue%2A> 方法。 <xref:System.Xml.Schema.XmlSchemaSet> 对象包含用于验证 XML 信息集的 XML 架构。 <xref:System.Xml.XmlNamespaceManager> 对象用于解析在验证期间遇到的命名空间。 <xref:System.Xml.Schema.XmlSchemaValidationFlags> 值用于禁用验证的某些功能。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> 构造函数的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

### <a name="initializing-validation"></a>初始化验证

构造了 <xref:System.Xml.Schema.XmlSchemaValidator> 对象之后，有两个重载 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法可用于初始化 <xref:System.Xml.Schema.XmlSchemaValidator> 对象的状态。 以下是这两个 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法。

- <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType>

- <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType>

默认的 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> 方法将 <xref:System.Xml.Schema.XmlSchemaValidator> 对象初始化为起始状态，使用 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> 作为参数的重载 <xref:System.Xml.Schema.XmlSchemaObject> 方法将 <xref:System.Xml.Schema.XmlSchemaValidator> 对象初始化为起始状态，以进行部分验证。

这两个 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法只能在构造了 <xref:System.Xml.Schema.XmlSchemaValidator> 对象之后或调用了 <xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A> 之后立即调用。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> 方法的示例，请参见简介中的示例。 有关 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

#### <a name="partial-validation"></a>部分验证

使用 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> 作为参数的 <xref:System.Xml.Schema.XmlSchemaObject> 方法将 <xref:System.Xml.Schema.XmlSchemaValidator> 对象初始化为起始状态，以进行部分验证。

在以下示例中，使用 <xref:System.Xml.Schema.XmlSchemaObject> 方法对 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A?displayProperty=nameWithType> 进行初始化，以进行部分验证。 在 `orderNumber` 对象的 <xref:System.Xml.XmlQualifiedName> 属性返回的 <xref:System.Xml.Schema.XmlSchemaObjectTable> 集合中，通过其中的 <xref:System.Xml.Schema.XmlSchemaSet.GlobalElements%2A> 选择架构元素，来传递 <xref:System.Xml.Schema.XmlSchemaSet> 架构元素。 然后，<xref:System.Xml.Schema.XmlSchemaValidator> 对象验证这个特定的元素。

```vb
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()
schemaSet.Add(Nothing, "schema.xsd")
schemaSet.Compile()
Dim nameTable As NameTable = New NameTable()
Dim manager As XmlNamespaceManager = New XmlNamespaceManager(nameTable)

Dim validator As XmlSchemaValidator = New XmlSchemaValidator(nameTable, schemaSet, manager, XmlSchemaValidationFlags.None)
validator.Initialize(schemaSet.GlobalElements.Item(New XmlQualifiedName("orderNumber")))

validator.ValidateElement("orderNumber", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateText("123")
validator.ValidateEndElement(Nothing)
```

```csharp
XmlSchemaSet schemaSet = new XmlSchemaSet();
schemaSet.Add(null, "schema.xsd");
schemaSet.Compile();
NameTable nameTable = new NameTable();
XmlNamespaceManager manager = new XmlNamespaceManager(nameTable);

XmlSchemaValidator validator = new XmlSchemaValidator(nameTable, schemaSet, manager, XmlSchemaValidationFlags.None);
validator.Initialize(schemaSet.GlobalElements[new XmlQualifiedName("orderNumber")]);

validator.ValidateElement("orderNumber", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateText("123");
validator.ValidateEndElement(null);
```

该示例使用以下 XML 架构作为输入。

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="orderNumber" type="xs:int" />
</xs:schema>
```

有关 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

### <a name="adding-additional-schemas"></a>添加其他架构

<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 类的 <xref:System.Xml.Schema.XmlSchemaValidator> 方法用于将 XML 架构添加到验证期间使用的架构集中。 <xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 方法可以用于模拟在所验证的 XML 信息集中遇到内联 XML 架构的效果。

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchema> 参数的目标命名空间无法与 <xref:System.Xml.Schema.XmlSchemaValidator> 对象已遇到的任何元素或属性的目标命名空间匹配。
>
> 如果 <xref:System.Xml.Schema.XmlSchemaValidationFlags.ProcessInlineSchema?displayProperty=nameWithType> 值未作为参数传递给 <xref:System.Xml.Schema.XmlSchemaValidator.%23ctor%2A> 构造函数，<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 方法不会执行任何操作。

<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 方法的结果取决于所验证的当前 XML 节点上下文。 有关验证上下文的更多信息，请参见本主题的“验证上下文”一节。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

### <a name="validating-elements-attributes-and-content"></a>验证元素、属性和内容

<xref:System.Xml.Schema.XmlSchemaValidator> 类提供多种方法用于针对 XML 架构验证 XML 信息集中的元素、属性和内容。 下表介绍每种方法。

|方法|描述|
|------------|-----------------|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>|在当前上下文中验证元素名。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>|在当前元素上下文中验证属性，或针对作为参数传递给 <xref:System.Xml.Schema.XmlSchemaAttribute> 方法的 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 对象验证属性。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>|验证元素上下文中所有必需的属性是否已存在，并且 <xref:System.Xml.Schema.XmlSchemaValidator> 对象是否已准备好验证元素的子内容。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>|验证当前元素上下文中是否允许包含文本，并累积文本，以验证当前元素是否包含简单内容。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>|验证当前元素上下文中是否允许包含空白，并累积空白，以验证当前元素是否包含简单内容。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>|对于包含简单内容的元素，验证元素的文本内容是否符合其数据类型，对于包含复杂内容的元素，验证当前元素的内容是否完整。|
|<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>|跳过当前元素内容的验证并准备 <xref:System.Xml.Schema.XmlSchemaValidator> 对象以验证父元素的上下文中的内容。|
|<xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|如果设置了 <xref:System.Xml.Schema.XmlSchemaValidationFlags.ProcessIdentityConstraints> 验证选项，结束验证并检查整个 XML 文档的标识约束。|

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator> 类包含已定义的状态转换，强制按顺序执行对上表中所述的每种方法的调用。 <xref:System.Xml.Schema.XmlSchemaValidator> 类特定的状态转换在本主题的“XmlSchemaValidator 状态转换”一节中介绍。

有关用于验证 XML 信息集中的元素、属性和内容的方法的示例，请参见上一节中的示例。 有关这些方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

#### <a name="validating-content-using-an-xmlvaluegetter"></a>使用 XmlValueGetter 编写内容

<xref:System.Xml.Schema.XmlValueGetter>`delegate` 可用于将属性、文本或空格节点的值传递为公共语言运行时 (CLR) 类型，与属性、文本或空格节点的 XML 架构定义语言 (XSD) 类型兼容。 如果属性、文本或空格节点的 CLR 值已可用，<xref:System.Xml.Schema.XmlValueGetter>`delegate` 非常有用，这样就免去了将它转换为 `string` 并重新分析以供验证的成本。

<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> 和 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> 方法是重载方法，使用属性、文本或空白节点的值作为 `string` 或 <xref:System.Xml.Schema.XmlValueGetter>`delegate`。

<xref:System.Xml.Schema.XmlSchemaValidator> 类的下列方法接受 <xref:System.Xml.Schema.XmlValueGetter>`delegate` 作为参数。

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>

下面是从简介中的 <xref:System.Xml.Schema.XmlValueGetter> 类示例获取的示例 `delegate`<xref:System.Xml.Schema.XmlSchemaValidator>。 <xref:System.Xml.Schema.XmlValueGetter>`delegate` 以 <xref:System.DateTime> 对象的形式返回属性的值。 要验证 <xref:System.DateTime> 返回的此 <xref:System.Xml.Schema.XmlValueGetter> 对象，<xref:System.Xml.Schema.XmlSchemaValidator> 对象先将其转换为属性数据类型的 ValueType（ValueType 是该 XSD 类型默认的 CLR 映射），然后检查转换后的值的各个方面。

```vb
Shared dateTimeGetterContent As Object

Shared Function DateTimeGetterHandle() As Object
    Return dateTimeGetterContent
End Function

Shared Function DateTimeGetter(dateTime As DateTime) As XmlValueGetter
    dateTimeGetterContent = dateTime
    Return New XmlValueGetter(AddressOf DateTimeGetterHandle)
End Function
```

```csharp
static object dateTimeGetterContent;

static object DateTimeGetterHandle()
{
    return dateTimeGetterContent;
}

static XmlValueGetter DateTimeGetter(DateTime dateTime)
{
    dateTimeGetterContent = dateTime;
    return new XmlValueGetter(dateTimeGetterHandle);
}
```

有关 <xref:System.Xml.Schema.XmlValueGetter>`delegate` 的完整示例，请参见简介中的示例。 若要详细了解 <xref:System.Xml.Schema.XmlValueGetter>`delegate`，请参阅 <xref:System.Xml.Schema.XmlValueGetter> 和 <xref:System.Xml.Schema.XmlSchemaValidator> 类的参考文档。

#### <a name="post-schema-validation-information"></a>后架构验证信息

<xref:System.Xml.Schema.XmlSchemaInfo> 类表示通过 <xref:System.Xml.Schema.XmlSchemaValidator> 类验证的 XML 节点的一些后架构验证信息。 <xref:System.Xml.Schema.XmlSchemaValidator> 类的各种方法使用 <xref:System.Xml.Schema.XmlSchemaInfo> 对象作为可选的 (`null`) `out` 参数。

在成功地验证之后，<xref:System.Xml.Schema.XmlSchemaInfo> 对象的属性将使用验证结果进行设置。 例如，使用 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> 方法成功地验证某个属性之后，<xref:System.Xml.Schema.XmlSchemaInfo> 对象的（如果指定）<xref:System.Xml.Schema.XmlSchemaInfo.SchemaAttribute%2A>、<xref:System.Xml.Schema.XmlSchemaInfo.SchemaType%2A>、<xref:System.Xml.Schema.XmlSchemaInfo.MemberType%2A> 和 <xref:System.Xml.Schema.XmlSchemaInfo.Validity%2A> 属性将使用验证结果进行设置。

下列 <xref:System.Xml.Schema.XmlSchemaValidator> 类方法使用 <xref:System.Xml.Schema.XmlSchemaInfo> 对象作为输出参数。

- <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>

- <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>

有关 <xref:System.Xml.Schema.XmlSchemaInfo> 类的完整示例，请参见简介中的示例。 有关 <xref:System.Xml.Schema.XmlSchemaInfo> 类的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaInfo> 类参考文档。

### <a name="retrieving-expected-particles-attributes-and-unspecified-default-attributes"></a>检索预计的粒子、属性和未指定的默认属性

<xref:System.Xml.Schema.XmlSchemaValidator> 类提供 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 和 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> 方法来检索当前验证上下文中预计的粒子、属性和未指定的默认属性。

#### <a name="retrieving-expected-particles"></a>检索预计的粒子

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法返回一个 <xref:System.Xml.Schema.XmlSchemaParticle> 对象数组，包含当前元素上下文中预计的粒子。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法可以返回的有效粒子是 <xref:System.Xml.Schema.XmlSchemaElement> 和 <xref:System.Xml.Schema.XmlSchemaAny> 类的实例。

如果内容模型的复合器是 `xs:sequence`，则只返回序列中的下一个粒子。 如果内容模型的复合器是 `xs:all` 或 `xs:choice`，则返回当前元素上下文中可能出现的所有有效粒子。

> [!NOTE]
> 如果 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法在调用 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法之后立即调用，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法将返回所有全局元素。

例如，在下面的 XML 架构定义语言 (XSD) 架构和 XML 文档中，在验证 `book` 元素之后，`book` 元素是当前的元素上下文。 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法返回一个数组，包含表示 <xref:System.Xml.Schema.XmlSchemaElement> 元素的单个 `title` 对象。 如果验证上下文是 `title` 元素，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法将返回一个空数组。 如果 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法在验证了 `title` 元素之后但是在验证 `description` 元素之前调用，将返回一个数组，包含表示 <xref:System.Xml.Schema.XmlSchemaElement> 元素的单个 `description` 对象。 如果 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法在验证了 `description` 元素之后调用，将返回一个数组，包含表示通配符的单个 <xref:System.Xml.Schema.XmlSchemaAny> 对象。

```vb
Dim reader As XmlReader =  XmlReader.Create("input.xml")

Dim schemaSet As New XmlSchemaSet()
schemaSet.Add(Nothing, "schema.xsd")
Dim manager As New XmlNamespaceManager(reader.NameTable)

Dim validator As New XmlSchemaValidator(reader.NameTable,schemaSet,manager,XmlSchemaValidationFlags.None)
validator.Initialize()

validator.ValidateElement("book", "", Nothing)

validator.ValidateEndOfAttributes(Nothing)
For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next

validator.ValidateElement("title", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next
validator.ValidateEndElement(Nothing)

For Each element As XmlSchemaElement In validator.GetExpectedParticles()
    Console.WriteLine(element.Name)
Next

validator.ValidateElement("description", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateEndElement(Nothing)

For Each particle As XmlSchemaParticle In validator.GetExpectedParticles()
    Console.WriteLine(particle.GetType())
Next

validator.ValidateElement("namespace", "", Nothing)
validator.ValidateEndOfAttributes(Nothing)
validator.ValidateEndElement(Nothing)

validator.ValidateEndElement(Nothing)
```

```csharp
XmlReader reader = XmlReader.Create("input.xml");

var schemaSet = new XmlSchemaSet();
schemaSet.Add(null, "schema.xsd");
var manager = new XmlNamespaceManager(reader.NameTable);

var validator = new XmlSchemaValidator(reader.NameTable, schemaSet, manager, XmlSchemaValidationFlags.None);
validator.Initialize();

validator.ValidateElement("book", "", null);

validator.ValidateEndOfAttributes(null);
foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}

validator.ValidateElement("title", "", null);
validator.ValidateEndOfAttributes(null);
foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}
validator.ValidateEndElement(null);

foreach (XmlSchemaElement element in validator.GetExpectedParticles())
{
    Console.WriteLine(element.Name);
}

validator.ValidateElement("description", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateEndElement(null);

foreach (XmlSchemaParticle particle in validator.GetExpectedParticles())
{
    Console.WriteLine(particle.GetType());
}

validator.ValidateElement("namespace", "", null);
validator.ValidateEndOfAttributes(null);
validator.ValidateEndElement(null);

validator.ValidateEndElement(null);
```

 该示例使用以下 XML 作为输入：

```xml
<xs:schema xmlns:xs="http://www.w3c.org/2001/XMLSchema">
  <xs:element name="book">
    <xs:sequence>
      <xs:element name="title" type="xs:string" />
      <xs:element name="description" type="xs:string" />
      <xs:any processContent="lax" maxOccurs="unbounded" />
    </xs:sequence>
  </xs:element>
</xs:schema>
```

该示例使用以下 XSD 架构作为输入：

```xml
<book>
  <title>My Book</title>
  <description>My Book's Description</description>
  <namespace>System.Xml.Schema</namespace>
</book>
```

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 类的 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 和 <xref:System.Xml.Schema.XmlSchemaValidator> 方法的结果取决于正在验证的当前上下文。 有关更多信息，请参见本主题的“验证上下文”一节。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法的示例，请参见简介中的示例。 有关 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

#### <a name="retrieving-expected-attributes"></a>检索预计的属性

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法返回一个 <xref:System.Xml.Schema.XmlSchemaAttribute> 对象数组，包含当前元素上下文中预计的属性。

例如，在简介的示例中，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法用于检索 `book` 元素的所有属性。

如果在调用 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法之后立即调用 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> 方法，将返回 XML 文档中可能出现的所有属性。 但是，如果在一次或多次调用 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法之后调用 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> 方法，将返回当前元素尚未进行验证的属性。

> [!NOTE]
> <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 类的 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 和 <xref:System.Xml.Schema.XmlSchemaValidator> 方法的结果取决于正在验证的当前上下文。 有关更多信息，请参见本主题的“验证上下文”一节。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法的示例，请参见简介中的示例。 有关 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

#### <a name="retrieving-unspecified-default-attributes"></a>检索未指定的默认属性

<xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> 方法使用元素上下文中以前尚未使用 <xref:System.Collections.ArrayList> 方法进行验证的默认值填充使用任何属性的 <xref:System.Xml.Schema.XmlSchemaAttribute> 对象指定的 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>。 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> 方法应在元素上下文中的每个属性上调用了 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> 方法之后调用。 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> 方法应用于确定要插入正在验证的 XML 文档的默认属性。

有关 <xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A> 方法的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaValidator> 类参考文档。

### <a name="handling-schema-validation-events"></a>处理架构验证事件

在验证过程中遇到的架构验证警告和错误由 <xref:System.Xml.Schema.XmlSchemaValidator.ValidationEventHandler> 类的 <xref:System.Xml.Schema.XmlSchemaValidator> 事件进行处理。

架构验证警告的 <xref:System.Xml.Schema.XmlSeverityType> 值为 <xref:System.Xml.Schema.XmlSeverityType.Warning>，架构验证错误的 <xref:System.Xml.Schema.XmlSeverityType> 值为 <xref:System.Xml.Schema.XmlSeverityType.Error>。 如果尚未分配任何 <xref:System.Xml.Schema.XmlSchemaValidator.ValidationEventHandler>，所有 <xref:System.Xml.Schema.XmlSchemaValidationException> 值为 <xref:System.Xml.Schema.XmlSeverityType> 的架构验证错误将引发 <xref:System.Xml.Schema.XmlSeverityType.Error>。 但是，<xref:System.Xml.Schema.XmlSchemaValidationException> 值为 <xref:System.Xml.Schema.XmlSeverityType> 的架构验证警告不会引发 <xref:System.Xml.Schema.XmlSeverityType.Warning>。

以下 <xref:System.Xml.Schema.ValidationEventHandler> 示例接收在架构验证过程中遇到的架构验证警告和错误，该示例从简介的示例中获取。

```vb
Shared Sub SchemaValidationEventHandler(sender As Object, e As ValidationEventArgs)

    Select Case e.Severity
        Case XmlSeverityType.Error
            Console.WriteLine(vbCrLf & "Error: {0}", e.Message)
            Exit Sub
        Case XmlSeverityType.Warning
            Console.WriteLine(vbCrLf & "Warning: {0}", e.Message)
            Exit Sub
    End Select
End Sub
```

```csharp
static void SchemaValidationEventHandler(object sender, ValidationEventArgs e)
{
    switch (e.Severity)
    {
        case XmlSeverityType.Error:
            Console.WriteLine("\nError: {0}", e.Message);
            break;
        case XmlSeverityType.Warning:
            Console.WriteLine("\nWarning: {0}", e.Message);
            break;
    }
}
```

有关 <xref:System.Xml.Schema.ValidationEventHandler> 的完整示例，请参见简介中的示例。 有关更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaInfo> 类参考文档。

## <a name="xmlschemavalidator-state-transition"></a>XmlSchemaValidator 状态转换

<xref:System.Xml.Schema.XmlSchemaValidator> 类包含已定义的状态转换，强制按顺序执行对用于验证 XML 信息集中的元素、属性和内容的每种方法的调用。

下表介绍 <xref:System.Xml.Schema.XmlSchemaValidator> 类的状态转换以及每个状态可以按顺序执行的方法调用。

|状态|切换|
|-----------|----------------|
|Validate|<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> (<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A> &#124; TopLevel*) <xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|
|TopLevel|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> &#124; <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> &#124; Element|
|元素|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>* (<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A> Content\*)? <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> &#124;<br /><br /> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>\* <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A> &#124;<br /><br /> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A> <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>\* <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A> Content\* <xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A> &#124;|
|内容|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A> &#124; <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> &#124; Element|

> [!NOTE]
> 如果调用方法的顺序不符合 <xref:System.InvalidOperationException> 对象的当前状态，上表中的每种方法将引发 <xref:System.Xml.Schema.XmlSchemaValidator>。

上面的状态转换表使用标点符号描述 <xref:System.Xml.Schema.XmlSchemaValidator> 类中状态转换的每种状态可以调用的方法和其他状态。 使用的符号与文档类型定义 (DTD) 的 XML 标准引用中出现的符号相同。

下表描述上面的状态转换表中出现的标点符号如何影响 <xref:System.Xml.Schema.XmlSchemaValidator> 类中状态转换的每种状态可以调用的方法和其他状态。

|符号|描述|
|------------|-----------------|
|&#124;|可以调用（竖线之前或竖线之后的）方法或状态。|
|?|问号之前的方法或状态是可选的，但是如果调用，只能调用一次。|
|\*|\* 符号之前的方法或状态是可选的，可以调用多次。|

## <a name="validation-context"></a>验证上下文

<xref:System.Xml.Schema.XmlSchemaValidator> 类中用于验证 XML 信息集中的元素、属性和内容的方法会更改 <xref:System.Xml.Schema.XmlSchemaValidator> 对象的验证上下文。 例如，<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A> 方法跳过当前元素内容的验证并准备 <xref:System.Xml.Schema.XmlSchemaValidator> 对象，以验证父元素上下文中的内容；这等效于跳过当前元素的所有子级的验证并调用 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> 方法。

<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 类的 <xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A>、<xref:System.Xml.Schema.XmlSchemaValidator.AddSchema%2A> 和 <xref:System.Xml.Schema.XmlSchemaValidator> 方法的结果取决于正在验证的当前上下文。

下表介绍在调用 <xref:System.Xml.Schema.XmlSchemaValidator> 类中的一个用于验证 XML 信息集中的元素、属性和内容的方法之后调用这些方法的结果。

|方法|GetExpectedParticles|GetExpectedAttributes|AddSchema|
|------------|--------------------------|---------------------------|---------------|
|<xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A>|如果调用默认的 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回包含所有全局元素的数组。<br /><br /> 如果调用使用 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 作为参数的重载 <xref:System.Xml.Schema.XmlSchemaObject> 方法来初始化元素的部分验证，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将只返回 <xref:System.Xml.Schema.XmlSchemaValidator> 对象初始化所针对的元素。|如果调用默认的 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 方法，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空数组。<br /><br /> 如果调用使用 <xref:System.Xml.Schema.XmlSchemaValidator.Initialize%2A> 作为参数的重载 <xref:System.Xml.Schema.XmlSchemaObject> 方法来初始化属性的部分验证，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将只返回 <xref:System.Xml.Schema.XmlSchemaValidator> 对象初始化所针对的属性。|如果没有预处理错误，将架构添加到 <xref:System.Xml.Schema.XmlSchemaSet> 对象的 <xref:System.Xml.Schema.XmlSchemaValidator> 中。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateElement%2A>|如果上下文元素有效，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回预计作为上下文元素子级的元素序列。<br /><br /> 如果上下文元素无效，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回一个空数组。|如果上下文元素有效，并且以前未调用过 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回上下文元素上定义的所有属性的列表。<br /><br /> 如果某些属性已进行验证，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回其他要验证的属性的列表。<br /><br /> 如果上下文元素无效，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空数组。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateAttribute%2A>|如果上下文属性是顶级属性，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回一个空数组。<br /><br /> 否则，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回预计作为上下文元素的第一个子级的元素序列。|如果上下文属性是顶级属性，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空数组。<br /><br /> 否则，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回其他要验证的属性的列表。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.GetUnspecifiedDefaultAttributes%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回需要作为上下文元素的第一个子级的元素的序列。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回仍需要验证的上下文元素的必选属性和可选属性的列表。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndOfAttributes%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回需要作为上下文元素的第一个子级的元素的序列。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空数组。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A>|如果上下文元素的 contentType 为 Mixed，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回下一个位置预计的元素序列。<br /><br /> 如果上下文元素的 contentType 为 TextOnly 或 Empty，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回一个空数组。<br /><br /> 如果上下文元素的 contentType 为 ElementOnly，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回下一个位置预计的、但是已出现验证错误的元素序列。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回未验证的上下文元素属性列表。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateWhitespace%2A>|如果上下文空白是顶级空白，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 将返回一个空数组。<br /><br /> 否则，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 方法的行为与 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> 中相同。|如果上下文空白是顶级空白，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空数组。<br /><br /> 否则，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 方法的行为与 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateText%2A> 中相同。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A>|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedParticles%2A> 返回上下文元素之后预计的元素序列（可能是同级）。|<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回未验证的上下文元素属性列表。<br /><br /> 如果上下文元素没有父级，<xref:System.Xml.Schema.XmlSchemaValidator.GetExpectedAttributes%2A> 将返回一个空列表（上下文元素是调用 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> 的当前元素的父级）。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.SkipToEndElement%2A>|与 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> 相同。|与 <xref:System.Xml.Schema.XmlSchemaValidator.ValidateEndElement%2A> 相同。|同上。|
|<xref:System.Xml.Schema.XmlSchemaValidator.EndValidation%2A>|返回一个空数组。|返回一个空数组。|同上。|

> [!NOTE]
> 调用上表中的任何方法时不会更改 <xref:System.Xml.Schema.XmlSchemaValidator> 类的各种属性返回的值。

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Schema.XmlSchemaValidator>
