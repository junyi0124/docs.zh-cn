---
title: XML 序列化详细信息
description: 序列化可将对象转换为可传输的窗体。 本文提供 XML 序列化和 XmlSerializer 类的概述。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, about XML serialization
- ICollection interface, serializing
- XmlSerializer class, serializing
- serialization, about serialization
- DataSet class, serializing
- XML Schema, serializing
ms.assetid: 8c63200d-db63-4a03-a93d-21641623df62
ms.openlocfilehash: 2971f5bbd587dabb62d095da3fef0b428ea9f039
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282280"
---
# <a name="xml-serialization"></a>XML 序列化

序列化是将对象转换成易于传输的形式的过程。 例如，可以序列化对象，并使用 HTTP 通过 Internet 在客户端和服务器之间进行传输。 另一方面，反序列化在流中重新构建对象。

 XML 序列化只将对象的公共字段和属性值序列化为 XML 流。 XML 序列化不包括类型信息。 例如，如果“Library”命名空间中存在“Book”对象，则不能保证将它反序列化为同一类型的对象 。

> [!NOTE]
> XML 序列化不能转换方法、索引器、私有字段或只读属性（只读集合除外）。 若要序列化对象的所有公共和私有字段和属性，请使用 <xref:System.Runtime.Serialization.DataContractSerializer> 而不要使用 XML 序列化。

 XML 序列化中的中心类是 <xref:System.Xml.Serialization.XmlSerializer> 类，此类中最重要的方法是 Serialize 和 Deserialize 方法 。 <xref:System.Xml.Serialization.XmlSerializer> 创建 C# 文件并将其编译为 .dll 文件，以执行此序列化。 [XML 序列化程序生成器工具 (Sgen.exe)](xml-serializer-generator-tool-sgen-exe.md) 旨在预先生成要与应用程序一起部署的这些序列化程序集，并改进启动性能。 XmlSerializer 生成的 XML 流符合万维网联合会 (W3C) [XML 架构定义语言 (XSD) 1.0 建议](https://www.w3.org/TR/xslt)。 而且，生成的数据类型符合文档“XML 架构第 2 部分：数据类型”。

 对象中的数据是用编程语言构造来描述的，如类、字段、属性、基元类型、数组，甚至是 XmlElement 或 XmlAttribute 对象形式的嵌入 XML 。 您可以创建自己的用特性批注的类，也可以使用 XML 架构定义工具生成基于现有 XML 架构的类。

 如果有 XML 架构，则可以运行 XML 架构定义工具生成一组类，将这组类的类型强制为此架构，并用特性进行批注。 当序列化这种类的实例时，生成的 XML 符合 XML 架构。 对于这种类，可以采用易于操作的对象模型进行编程，同时确保生成的 XML 符合 XML 架构。 这是使用 .NET 中的其他类（如 XmlReader 和 XmlWriter 类）分析和编写 XML 流的另一种方法 。 有关详细信息，请参阅 [XML 文档和数据](../data/xml/index.md)。 这些类可让您分析任何 XML 流。 相反，如果 XML 流应符合已知的 XML 架构，则使用 XmlSerializer。

 特性可控制 XmlSerializer 类生成的 XML 流，使你能够设置 XML 流的 XML 命名空间、元素名称、特性名称等。 有关这些特性以及它们如何控制 XML 序列化的详细信息，请参阅[使用特性控制 XML 序列化](controlling-xml-serialization-using-attributes.md)。 有关那些用于控制生成的 XML 的特性的表格，请参阅[控制 XML 序列化的特性](attributes-that-control-xml-serialization.md)。

 XmlSerializer 类可以进一步序列化对象并生成编码的 SOAP XML 流。 生成的 XML 符合万维网联合会文档“简单对象访问协议 (SOAP) 1.1”(Simple Object Access Protocol (SOAP) 1.1) 的第 5 节。 有关此进程的详细信息，请参阅[如何：将对象序列化为 SOAP 编码的 XML 流](how-to-serialize-an-object-as-a-soap-encoded-xml-stream.md)。 有关可控制生成的 XML 的特性的表格，请参阅[控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)。

 XmlSerializer 类生成由 XML Web 服务创建并传递给 XML Web 服务的 SOAP 信息。 要控制 SOAP 消息，可以将特性应用于在 XML Web services 文件 (.asmx) 中找到的类、返回值、参数和字段。 由于 XML Web services 可以使用文本或编码的 SOAP 样式，因此既可以使用“用来控制 XML 序列化的特性”中列出的特性，也可以使用“用来控制编码的 SOAP 序列化的特性”中列出的特性。 有关使用特性来控制由 XML Web 服务生成的 XML 的详细信息，请参阅[使用 XML Web 服务进行 XML 序列化](xml-serialization-with-xml-web-services.md)。 有关 SOAP 和 XML Web 服务的详细信息，请参阅[自定义 SOAP 消息格式](/previous-versions/dotnet/netframework-4.0/dkwy2d72(v=vs.100))。

## <a name="security-considerations-for-xmlserializer-applications"></a>XmlSerializer 应用程序的安全注意事项

创建使用 XmlSerializer 的应用程序时，应了解以下各项及其含义：

- XmlSerializer 在由 TEMP 环境变量命名的目录中创建 C# (.cs) 文件并将它们编译为 dll 文件；这些 DLL 文件将进行序列化。

  > [!NOTE]
  > 可预先生成这些序列化程序集，并使用 SGen.exe 工具对它们进行签名。 这不适用于 Web 服务的服务器。 也就是说，这只适用于客户端用法和手动序列化。

  代码和 DLL 在创建和编译时易受恶意进程破坏。 当使用的计算机运行 Microsoft Windows NT 4.0 或更高版本时，可能有两个或多个用户共享 TEMP 目录。 如果两个帐户的安全权限不同，并且权限较高的帐户使用 XmlSerializer 运行应用程序，则共享 TEMP 目录是危险的操作。 在这种情况下，一个用户可能因替换已编译的 .cs 或 .dll 文件而违反计算机的安全性。 要消除这一问题，应始终确保计算机上的每个帐户具有各自的配置文件。 默认情况下，TEMP 环境变量为每个帐户指向不同的目录。

- 如果恶意用户将连续的 XML 数据流发送至 Web 服务器（拒绝服务攻击），则 XmlSerializer 会继续处理数据，直到计算机资源不足为止。

  如果您使用的计算机运行了 Internet 信息服务 (IIS)，并且您的应用程序在 IIS 内运行，则可消除这种攻击。 IIS 提供了一种网关，这种网关不处理长度超过设定量（默认值为 4 KB）的流。 如果创建的应用程序不使用 IIS 并且使用 XmlSerializer 进行反序列化，则应该实现防止拒绝服务攻击的类似网关。

- XmlSerializer 使用提供给它的任何类型序列化数据并运行任何代码。

  恶意对象造成威胁的方式有两种。 它可以运行恶意代码，也可以将恶意代码插入 XmlSerializer 创建的 C# 文件中。 在第一种情况下，如果恶意对象试图运行破坏性的过程，代码访问安全有助于防止发生任何损坏。 在第二种情况下，从理论上来说，恶意对象有可能以某种方式将代码插入 XmlSerializer 创建的 C# 文件中。 尽管这一问题已得到彻底检查并且认为不可能发生这种攻击，但还是应当采取防范措施，一定不要使用未知和不受信任的类型来序列化数据。

- 已序列化的敏感数据可能容易受到攻击。

  XmlSerializer 序列化数据之后，数据可存储为 XML 文件或其他数据存储。 如果数据存储区可供其他进程使用或者可在 Intranet 或 Internet 上看见，则数据可能被盗和恶意使用。 例如，如果您创建一个应用程序来序列化包含信用卡号码的订单，则数据是高度敏感的。 为防止被盗和恶意使用，应始终保护您的数据存储区并采取步骤保持其私密性。

## <a name="serialization-of-a-simple-class"></a>简单类的序列化

下面的代码示例显示具有公共字段的基类。

```vb
Public Class OrderForm
    Public OrderDate As DateTime
End Class
```

```csharp
public class OrderForm
{
    public DateTime OrderDate;
}
```

此类的实例序列化后可能如下所示。

```xml
<OrderForm>
    <OrderDate>12/12/01</OrderDate>
</OrderForm>
```

有关序列化的更多示例，请参阅 [XML 序列化示例](examples-of-xml-serialization.md)。

## <a name="items-that-can-be-serialized"></a>可序列化的项

可使用 XmlSerializer 类对以下各项进行序列化：

- 公共类的公共读/写属性和字段。

- 执行 ICollection 或 IEnumerable 的类 。

  > [!NOTE]
  > 仅序列化集合，不序列化公共属性。

- XmlElement 对象。

- XmlNode 对象。

- DataSet 对象。

 有关序列化或反序列化对象的更多信息，请参阅[如何：序列化对象](how-to-serialize-an-object.md)和[如何：反序列化对象](how-to-deserialize-an-object.md)。

## <a name="advantages-of-using-xml-serialization"></a>使用 XML 序列化的优点

将对象序列化为 XML 时，XmlSerializer 类可提供完整而灵活的控制。 如果要创建 XML Web services，可以将控制序列化的特性应用于类和成员，以确保 XML 输出符合特定架构。

例如，XmlSerializer 使你能够：

- 指定字段或属性是否应编码为特性或元素。

- 指定要使用的 XML 命名空间。

- 指定字段或属性名称不恰当时的元素或特性名称。

XML 序列化的另一个优点是，只要生成的 XML 流符合给定的架构，就对开发的应用程序没有任何约束。 假设有一个架构，它用于描述书籍。 它提供有书名、作者、出版商和 ISBN 号元素。 您可以开发一个应用程序来以任何想要的方式（例如以书籍订单或书籍库存方式）处理 XML 数据。 在任一种情况下，唯一的要求是 XML 流符合指定的 XML 架构定义语言 (XSD) 架构。

## <a name="xml-serialization-considerations"></a>XML 序列化注意事项

使用 XmlSerializer 类时，应注意以下事项：

- Sgen.exe 工具特别设计为生成序列化程序集，以获得最佳性能。

- 序列化数据只包含数据本身和类的结构。 类型标识和程序集信息不包括在内。

- 只能序列化公共属性和字段。 属性必须具有公共访问器（get 和 set 方法）。 如果必须序列化非公共数据，请使用 <xref:System.Runtime.Serialization.DataContractSerializer> 类而不使用 XML 序列化。

- 类必须具有无参数构造函数才能被 XmlSerializer 序列化。

- 方法不能被序列化。

- 如下所述，如果实现 IEnumerable 或 ICollection 的类满足某些要求，XmlSerializer 则可以处理这些类  。

  实现 IEnumerable 的类必须实现采用单个参数的公共 Add 方法 。 Add 方法的参数必须与从“IEnumerator.Current”属性返回的类型一致（多态），该属性是从 GetEnumerator 方法返回的  。

  除了实现 IEnumerable 之外，还能实现 ICollection 的类（如 CollectionBase）必须具有采用整型的公共“Item”索引属性（在 C# 中为索引器），而且它必须有一个“integer”类型的公共“Count”属性     。 传递给 Add 方法的参数必须与从“Item”属性返回的类型相同，或者为此类型的基之一 。

  对于实现 ICollection 的类，可从已编制索引的“Item”属性检索要序列化的值，而不是通过调用 GetEnumerator 进行检索  。 此外，除了返回另一个集合类（实现 ICollection 的一个类）的公共字段外，公共字段和属性不会被序列化。 有关示例，请参阅 [XML 序列化示例](examples-of-xml-serialization.md)。

## <a name="xsd-data-type-mapping"></a>XSD 数据类型映射

标题为 [XML 架构第 2 部分：数据类型](https://www.w3.org/TR/xmlschema-2/)的 W3C 文档指定 XML 架构定义语言 (XSD) 架构中允许的简单数据类型。 对于其中的许多数据类型（如“int”和“decimal”），.NET 中都有相应的数据类型 。 然而，某些 XML 数据类型没有相应的 .NET 数据类型（如“NMTOKEN”数据类型）。 在这种情况下，如果使用 XML 架构定义工具（[XML 架构定义工具 (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md)）从架构中生成类，相应的特性会应用到字符串类型的成员，其“DataType”属性会设置为 XML 数据类型名称。 例如，如果架构包含 XML 数据类型为“NMTOKEN”且名为“MyToken”的元素，则生成的类可能包含下例中所示的成员。

```vb
<XmlElement(DataType:="NMTOKEN")> _
Public MyToken As String
```

```csharp
[XmlElement(DataType = "NMTOKEN")]
public string MyToken;
```

同样，如果要创建的类必须符合特定的 XML 架构 (XSD)，则应该应用相应的特性并且将其“DataType”属性设置为所需的 XML 数据类型名称。

有关类型映射的完整列表，请参阅以下任一特性类的“DataType”属性：

- <xref:System.Xml.Serialization.SoapAttributeAttribute>

- <xref:System.Xml.Serialization.SoapElementAttribute>

- <xref:System.Xml.Serialization.XmlArrayItemAttribute>

- <xref:System.Xml.Serialization.XmlAttributeAttribute>

- <xref:System.Xml.Serialization.XmlElementAttribute>

- <xref:System.Xml.Serialization.XmlRootAttribute>

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlSerializer>
- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.IO.FileStream>
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [二进制序列化](binary-serialization.md)
- [序列化](index.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [XML 序列化示例](examples-of-xml-serialization.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
