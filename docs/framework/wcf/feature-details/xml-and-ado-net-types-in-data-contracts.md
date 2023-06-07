---
title: 数据协定中的 XML 和 ADO.NET 类型
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c2ce8461-3c15-4c41-8c81-1cb78f5b59a6
ms.openlocfilehash: d4ac956af0addf9c3b38f3bfb8e8644757dc81c3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238302"
---
# <a name="xml-and-adonet-types-in-data-contracts"></a>数据协定中的 XML 和 ADO.NET 类型

Windows Communication Foundation (WCF) 数据协定模型支持直接表示 XML 的特定类型。 当这些类型序列化为 XML 时，序列化程序将写出这些类型的 XML 内容，而不再进一步进行任何处理。 支持的类型为 <xref:System.Xml.XmlElement>、<xref:System.Xml.XmlNode> 的数组（但不是 `XmlNode` 类型本身）以及实现 <xref:System.Xml.Serialization.IXmlSerializable> 的类型。 <xref:System.Data.DataSet> 和 <xref:System.Data.DataTable> 类型以及类型化数据集通常用于数据库编程。 这些类型可实现 `IXmlSerializable` 接口，因此它们在数据协定模型中可序列化。 本主题的结尾还列出了一些有关这些类型的特殊注意事项。  
  
## <a name="xml-types"></a>XML 类型  
  
### <a name="xml-element"></a>XmlElement  

 `XmlElement` 类型使用其 XML 内容进行序列化。 例如，使用以下类型。  
  
 [!code-csharp[DataContractAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#4)]
 [!code-vb[DataContractAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#4)]  
  
 它将序列化为 XML，如下所示：  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
    <myDataMember>  
        <myElement xmlns="" myAttribute="myValue">  
            myContents  
        </myElement>  
    </myDataMember>  
</MyDataContract>  
```  
  
 请注意，包装数据成员元素 `<myDataMember>` 仍然存在。 无法在数据协定模型中移除此元素。 处理此模型的序列化程序（<xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.NetDataContractSerializer>）可以将特殊属性发出到此包装元素。 这些属性包括标准 XML 架构实例“nil”属性（允许 `XmlElement` 为 `null`）和“Type”属性（允许以多元方式使用 `XmlElement`）。 此外，以下 XML 特性特定于 WCF： "Id"、"Ref"、"Type" 和 "Assembly"。 可以发出这些属性以支持与已启用的对象图保留模式或 `XmlElement` 一起使用 <xref:System.Runtime.Serialization.NetDataContractSerializer>。  (有关对象关系图保留模式的详细信息，请参阅 [序列化和反序列化](serialization-and-deserialization.md)。 )   
  
 允许使用 `XmlElement` 的数组或集合，并且可以将它们作为任何其他数组或集合进行处理。 也就是说，将有一个包装元素适用于整个集合，并且数组中的每一个 `<myDataMember>` 都对应一个单独的包装元素（类似于上面示例中的 `XmlElement`）。  
  
 在反序列化过程中，反序列化程序将从传入的 XML 创建一个 `XmlElement`。 反序列化程序还将提供一个有效的父 <xref:System.Xml.XmlDocument>。  
  
 请确保被反序列化为 `XmlElement` 的 XML 片段定义了它使用的所有前缀，并且不依赖于上级元素的任何前缀定义。 仅在使用 `DataContractSerializer` 以访问来自不同（非 `DataContractSerializer`）源的 XML 时需要注意此事项。  
  
 当与一起使用时 `DataContractSerializer` ， `XmlElement` 可以将以多元方式分配给类型的数据成员 <xref:System.Object> 。 即使 <xref:System.Collections.IEnumerable> 实现 `XmlElement`，它也不能用作集合类型，并且无法分配给 <xref:System.Collections.IEnumerable> 数据成员。 与所有多态分配一样，将在 `DataContractSerializer` 生成的 XML 中发出数据协定名称，在这种情况下，它是 "" 命名空间中的 "XmlElement" http://schemas.datacontract.org/2004/07/System.Xml 。  
  
 使用 `NetDataContractSerializer` 时，支持任何有效的 `XmlElement` 多态分配（分配给 `Object` 或 `IEnumerable`）。  
  
 请勿尝试使用任何一个从 `XmlElement` 派生的类型的序列化程序，无论是否对它们进行多元方式的分配。  
  
### <a name="array-of-xmlnode"></a>XmlNode 数组  

 <xref:System.Xml.XmlNode> 数组的用法与 `XmlElement` 的用法极其类似。 但 `XmlNode` 数组的用法比 `XmlElement` 的用法更灵活。 您可以在数据成员包装元素内写入多个元素。 还可以在数据成员包装元素内插入元素以外的内容，如 XML 注释。 最后，您可以向包装数据成员元素中插入属性。 所有这些操作都可以通过对 `XmlNode` 的数组填充 `XmlNode` 的特定派生类来实现，如 <xref:System.Xml.XmlAttribute>、`XmlElement` 或 <xref:System.Xml.XmlComment>。 例如，使用以下类型。  
  
 [!code-csharp[DataContractAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#5)]
 [!code-vb[DataContractAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#5)]  
  
 序列化后，所生成的 XML 将类似于以下代码。  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
  <myDataMember myAttribute="myValue">  
     <!--myComment-->  
     <myElement xmlns="" myAttribute="myValue">  
 myContents  
     </myElement>  
     <myElement xmlns="" myAttribute="myValue">  
       myContents  
     </myElement>  
  </myDataMember>  
</MyDataContract>  
```  
  
 请注意，数据成员包装元素 `<myDataMember>` 包含一个属性、一个注释和两个元素。 这些是经过序列化的四个 `XmlNode` 实例。  
  
 产生无效 XML 的 `XmlNode` 数组无法序列化。 例如，两个 `XmlNode` 实例（其中一个是 `XmlElement`，另一个是 <xref:System.Xml.XmlAttribute>）的数组是无效的，因为此序列与任何有效的 XML 实例都不对应（没有附加该属性的位置）。  
  
 在反序列化 `XmlNode` 数组时，将创建节点并用传入 XML 的信息进行填充。 反序列化程序还将提供一个有效的父 <xref:System.Xml.XmlDocument>。 所有节点都将反序列化，包括包装数据成员元素上的任何属性，但不包括 WCF 序列化程序所放置的属性 (如用于指示多态赋值) 的属性。 有关在 XML 片段中定义所有命名空间前缀的注意事项适用于反序列化 `XmlNode` 数组，它们所起的作用与在反序列化 `XmlElement` 时类似。  
  
 如果在启用对象图预留时使用序列化程序，则对象相等仅预留在 `XmlNode` 数组级别上，而不是各个 `XmlNode` 实例上。  
  
 请勿尝试序列化其中有一个或多个节点设置为 `XmlNode` 的 `null` 数组。 允许将整个数组成员都设置为 `null`，但是数组中包含的任何单个 `XmlNode` 除外。 如果整个数组成员都为 Null，则包装数据成员元素将包含一个指示该数组为 Null 的特殊属性。 在反序列化时，整个数组成员也将变为 Null。  
  
 序列化程序仅对 `XmlNode` 的常规数组进行特殊处理。 不会对声明为包含 `XmlNode` 的其他集合类型的数据成员，或声明为类型派生自 `XmlNode` 的数组的数据成员进行特殊处理。 因此，除非它们还满足其他某个序列化条件，否则一般情况下，它们都不能序列化。  
  
 允许使用数组或 `XmlNode` 数组的集合。 有一个包装元素适用于整个集合，并且外部数组或集合中的每一个 `<myDataMember>` 数组都对应一个单独的包装元素（类似于上面示例中的 `XmlNode`）。  
  
 使用 <xref:System.Array> 实例填充 `Object` 的 `Array` 类型的数据成员（或 `IEnumerable` 的 `XmlNode` 类型的数据成员）时，将不会导致数据成员被视为 `Array` 实例的 `XmlNode`。 将分别序列化每个数组成员。  
  
 当与 `DataContractSerializer` 一起使用时，能够以多元方式分配 `XmlNode` 的数组，但只能分配到 `Object` 类型的数据成员。 即使 `IEnumerable` 数组实现 `XmlNode`，它也无法用作集合类型，并且无法分配给 `IEnumerable` 数据成员。 与所有多态分配一样，将在 `DataContractSerializer` 生成的 XML 中发出数据协定名称–在本例中，它是 "" 命名空间中的 "ArrayOfXmlNode" http://schemas.datacontract.org/2004/07/System.Xml 。 当与一起使用时 `NetDataContractSerializer` ，支持任何有效的 `XmlNode` 数组赋值。  
  
### <a name="schema-considerations"></a>架构注意事项  

 有关 XML 类型的架构映射的详细信息，请参阅 [数据协定架构引用](data-contract-schema-reference.md)。 本节介绍重点摘要。  
  
 将 `XmlElement` 类型的数据成员映射到使用以下匿名类型定义的元素。  
  
```xml  
<xsd:complexType>  
   <xsd:sequence>  
      <xsd:any minOccurs="0" processContents="lax" />  
   </xsd:sequence>  
</xsd:complexType>  
```  
  
 将 `XmlNode` 数组类型的数据成员映射到使用以下匿名类型定义的元素。  
  
```xml  
<xsd:complexType mixed="true">  
   <xsd:sequence>  
      <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax" />  
   </xsd:sequence>  
   <xsd:anyAttribute/>  
</xsd:complexType>  
```  
  
## <a name="types-implementing-the-ixmlserializable-interface"></a>实现 IXmlSerializable 接口的类型  

 `IXmlSerializable` 完全支持实现 `DataContractSerializer` 接口的类型。 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性应始终应用于这些类型以控制它们的架构。  
  
 实现 `IXmlSerializable` 的类型有以下三种：表示任意内容的类型、表示单一元素的类型和旧的 <xref:System.Data.DataSet> 类型。  
  
- 内容类型使用由 `XmlSchemaProviderAttribute` 属性指定的架构提供程序方法。 该方法不返回 `null`，并且属性 (Attribute) 上的 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> 属性 (Property) 将保留为它的默认值 `false`。 这是 `IXmlSerializable` 类型的最常见的用法。  
  
- 当 `IXmlSerializable` 类型必须控制其根元素名称时，使用元素类型。 若要将某个类型标记为元素类型，可以将 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> 属性 (Attribute) 上的 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性 (Property) 设置为 `true`，或从架构提供程序方法返回 null。 元素类型可以具有架构提供程序方法，在 `XmlSchemaProviderAttribute` 中可以指定 null 代替方法名称。 但是，如果 `IsAny` 为 `true`，并且已指定了架构提供程序方法，则该方法必须返回 null。  
  
- 旧 <xref:System.Data.DataSet> 类型为没有使用 `IXmlSerializable` 特性标记的 `XmlSchemaProviderAttribute` 类型。 相反，它们依赖于架构生成的 <xref:System.Xml.Serialization.IXmlSerializable.GetSchema%2A> 方法。 此模式用于 `DataSet` 类型，其类型化数据集在 .NET Framework 的早期版本中派生了一个类，但现在它已过时，并且只有旧版本才支持它。 不要依赖于此模式，并始终将 `XmlSchemaProviderAttribute` 应用于 `IXmlSerializable` 类型。  
  
### <a name="ixmlserializable-content-types"></a>IXmlSerializable 内容类型  

 如果正在序列化实现 `IXmlSerializable` 的类型的数据成员，且该类型为前面定义的内容类型，则序列化程序将编写该数据成员的包装元素，并将控制权传递给 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 方法。 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 实现可以编写任何 XML，包括将属性添加到包装元素。 `WriteXml` 完成后，序列化程序将关闭该元素。  
  
 如果正在反序列化实现 `IXmlSerializable` 的类型的数据成员，且该类型为前面定义的内容类型，则反序列化程序会将 XML 读取器放置在该数据成员的包装元素上，并将控制权传递给 <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> 方法。 该方法必须读取整个元素，包括起始和结束标记。 请确保你的 `ReadXml` 代码可处理元素为空的情况。 此外，您的 `ReadXml` 实现也不应该依赖于以特殊方式进行命名的包装元素。 序列化程序所选的名称可以不同。  
  
 允许以多元方式分配 `IXmlSerializable` 内容类型，例如，分配给 <xref:System.Object> 类型的数据成员。 还允许类型实例为 null。 最后，可以在启用对象图保留的情况下使用 `IXmlSerializable` 类型，以及和 <xref:System.Runtime.Serialization.NetDataContractSerializer> 一起使用。 所有这些功能都要求 WCF 序列化程序将某些属性附加到包装元素中， ( "nil" 和 "type"，在 XML 架构实例命名空间中，将 "Id"、"Ref"、"Type" 和 "Assembly" 附加到 WCF 特定的命名空间) 。  
  
#### <a name="attributes-to-ignore-when-implementing-readxml"></a>实现 ReadXml 时要忽略的属性  

 在将控制权传递给 `ReadXml` 代码之前，反序列化程序将检查 XML 元素、检测这些特殊的 XML 属性，以及对它们进行操作。 例如，如果“nil”为 `true`，则将反序列化一个 Null 值，并且不调用 `ReadXml`。 如果检测到多态性，则将反序列化该元素的内容，就好像该元素为其他类型一样。 调用以多元方式分配的类型的 `ReadXml` 实现。 在任何情况下，`ReadXml` 实现都应忽略这些特殊属性，因为它们均由反序列化程序处理。  
  
### <a name="schema-considerations-for-ixmlserializable-content-types"></a>IXmlSerializable 内容类型的架构注意事项  

 导出 `IXmlSerializable` 内容类型的架构时，将调用架构提供程序方法。 并将 <xref:System.Xml.Schema.XmlSchemaSet> 传递给架构提供程序方法。 该方法可以将任何有效架构添加到架构集中。 该架构集将包含发生架构导出时已知的架构。 当架构提供程序方法必须将项添加到架构集中时，它必须确定该集中是否已存在具有适当命名空间的 <xref:System.Xml.Schema.XmlSchema>。 如果已存在，则架构提供程序方法必须将新项添加到现有 `XmlSchema` 中。 否则，它必须创建一个新的 `XmlSchema` 实例。 如果使用的是 `IXmlSerializable` 类型数组，这是非常重要的。 例如，如果您有一个 `IXmlSerializable` 类型，且已作为命名空间“B”中的类型“A”导出，则在调用架构提供程序方法时，架构集可能已包含保存“ArrayOfA”类型的“B”架构。  
  
 除了将类型添加到 <xref:System.Xml.Schema.XmlSchemaSet>，内容类型的架构提供程序方法必须返回非空值。 它可能返回 <xref:System.Xml.XmlQualifiedName>，指定用于给定 `IXmlSerializable` 类型的架构类型名称。 此限定名称也将用作数据协定名称和该类型的命名空间。 返回架构提供程序方法时允许立即返回架构集中不存在的类型。 但是，导出所有相关类型（调用 <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> 方法中有关 <xref:System.Runtime.Serialization.XsdDataContractExporter> 的所有相关类型并访问 <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> 属性）时，该类型应该存在于架构集中。 在进行所有相关的 `Schemas` 调用之前访问 `Export` 属性可能会导致 <xref:System.Xml.Schema.XmlSchemaException>。 有关导出过程的详细信息，请参阅 [从类导出架构](exporting-schemas-from-classes.md)。  
  
 架构提供程序方法也可以返回要使用的 <xref:System.Xml.Schema.XmlSchemaType>。 该类型可能是匿名的，也可能不是。 如果它是匿名的，则每次将 `IXmlSerializable` 类型用作数据成员时，`IXmlSerializable` 类型的架构都将作为匿名类型导出。 `IXmlSerializable` 类型仍具有数据协定名称和命名空间。  (根据[数据协定名称](data-contract-names.md)中所述确定此项，但 <xref:System.Runtime.Serialization.DataContractAttribute> 不能使用该特性自定义该名称。 ) 如果不是匿名的，则它必须是中的类型之一。 `XmlSchemaSet` 这种情况相当于返回该类型的 `XmlQualifiedName`。  
  
 另外，还将导出该类型的全局元素声明。 如果该类型没有应用于它的 <xref:System.Xml.Serialization.XmlRootAttribute> 属性 (Attribute)，则该元素将具有与数据协定相同的名称和命名空间，且它的“nillable”属性 (property) 将为 true。 唯一的例外情况是架构命名空间 ( " http://www.w3.org/2001/XMLSchema " ) –如果该类型的数据协定在此命名空间中，则对应的全局元素位于空白命名空间中，因为禁止将新元素添加到架构命名空间。 如果该类型具有应用于它的 `XmlRootAttribute` 属性 (Attribute)，则将使用以下属性 (Property) 导出全局元素声明：<xref:System.Xml.Serialization.XmlRootAttribute.ElementName%2A>、<xref:System.Xml.Serialization.XmlRootAttribute.Namespace%2A> 和 <xref:System.Xml.Serialization.XmlRootAttribute.IsNullable%2A>。 应用 `XmlRootAttribute` 的默认值为数据协定名称、空白命名空间和值为 true 的“nillable”。  
  
 同样的全局元素声明规则也适用于旧数据集类型。 请注意，`XmlRootAttribute` 无法重写通过自定义代码添加的全局元素声明，无论是使用构架提供程序方法添加到 `XmlSchemaSet` 中的，还是通过旧数据集类型的 `GetSchema` 添加的。  
  
### <a name="ixmlserializable-element-types"></a>IXmlSerializable 元素类型  

 `IXmlSerializable` 元素类型将 `IsAny` 属性设为 `true`，或使它们的架构提供程序方法返回 `null`。  
  
 序列化和反序列化元素类型与序列化和反序列化内容类型极其相似。 但是，也有一些重要的区别：  
  
- `WriteXml` 实现应只写入一个元素（当然，该元素可包含多个子元素）。 它不应是此单个元素、多个同级元素或混合内容以外的写属性。 该元素可能为空。  
  
- `ReadXml` 实现不应读取包装元素。 它应读取 `WriteXml` 生成的那一个元素。  
  
- 定期序列化元素类型时（例如，作为数据协定中的数据成员），序列化程序将在调用 `WriteXml` 之前输出包装元素，如同处理内容类型一样。 然而，在顶层序列化元素类型时，序列化程序通常根本不输出 `WriteXml` 写入的元素周围的包装元素，除非在 `DataContractSerializer` 或 `NetDataContractSerializer` 构造函数中构造序列化程序时显式指定了根名称和命名空间。 有关详细信息，请参阅 [序列化和反序列](serialization-and-deserialization.md)化。  
  
- 如果在构造期间没有指定根名称和命名空间的情况下在顶层序列化元素类型，则 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> 和 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> 实质上不执行任何操作，且 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A> 将调用 `WriteXml`。 在此模式下，正在序列化的对象不能为 null，也不能多元分配。 另外，不能启用对象图保留，也不能使用 `NetDataContractSerializer`。  
  
- 如果在构造时没有指定根名称和命名空间的情况下在顶级反序列化某一元素类型，则在找到任何元素的开头时，<xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> 将返回 `true`。 <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> 参数设置为 `verifyObjectName` 的 `true` 在行为上与实际读取该对象前 `IsStartObject` 的行为相同。 然后，`ReadObject` 将控制传递到 `ReadXml` 方法。  
  
 如上节所述，元素类型的导出架构与 `XmlElement` 类型的导出架构相同，只是架构提供程序方法可以像处理内容类型那样将任何其他架构添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中。 不允许使用元素类型的 `XmlRootAttribute` 属性，也从不为这些类型发出全局元素声明。  
  
### <a name="differences-from-the-xmlserializer"></a>与 XmlSerializer 的区别  

 `IXmlSerializable` 接口以及 `XmlSchemaProviderAttribute` 和 `XmlRootAttribute` 属性也都可以由 <xref:System.Xml.Serialization.XmlSerializer> 理解。 但是，在数据协定模型中处理它们的方法有一些不同。 主要的区别总结如下：  
  
- 架构提供程序方法在 `XmlSerializer` 中必须公开为可用的，但在数据协定模型中无需如此。  
  
- 如果 `IsAny` 在数据协定模型中为 true，但在 `XmlSerializer` 中不为 true，则调用架构提供程序方法。  
  
- 如果内容或旧数据集类型中没有 `XmlRootAttribute` 属性，则 `XmlSerializer` 将在空白命名空间中导出全局元素声明。 如前面所述，在数据协定模型中，使用的命名空间通常是数据协定命名空间。  
  
 创建与这两种序列化技术一起使用的类型时，请注意这些区别。  
  
### <a name="importing-ixmlserializable-schema"></a>导入 IXmlSerializable 架构  

 导入从 `IXmlSerializable` 类型生成的架构时，有以下几种可能：  
  
- 生成的架构可能是有效的数据协定架构，如 [数据协定架构引用](data-contract-schema-reference.md)中所述。 在此情况下，架构可以正常导入，并且将生成常规数据协定类型。  
  
- 生成的架构可能不是有效的数据协定架构。 例如，您的架构提供程序方法可能生成包含 XML 属性的架构，而数据协定模型不支持 XML 属性。 在此情况下，您可以将架构作为 `IXmlSerializable` 类型导入。 默认情况下，此导入模式不处于启用状态，但可以轻松启用–例如，通过 `/importXmlTypes` 命令行开关转到 [)  ( ](../servicemodel-metadata-utility-tool-svcutil-exe.md)"带 [导入架构以生成类中对](importing-schema-to-generate-classes.md)此进行了详细介绍。 请注意，您必须直接处理您的类型实例的 XML。 您也可以考虑使用其他支持各种架构的不同序列化技术 – 请参见有关使用 `XmlSerializer` 的主题。  
  
- 您可能希望重新使用代理中的现有 `IXmlSerializable` 类型，而不生成新的类型。 在此情况下，“导入架构以生成类型”主题中介绍的引用类型功能可用于指示要重新使用的类型。 这对应于在 svcutil.exe 上使用 `/reference` 开关，以指定包含要重新使用的类型的程序集。  
  
## <a name="representing-arbitrary-xml-in-data-contracts"></a>在数据协定中表示任意 XML  

 `XmlElement`、`XmlNode` 数组和 `IXmlSerializable` 类型，允许您将插入任意 XML 插入数据协定模型中。 `DataContractSerializer` 和 `NetDataContractSerializer` 将此 XML 内容传递到所使用的 XML 编写器，而不会干扰此过程。 但是，XML 编写器可能对它们编写的 XML 强制执行一些限制。 具体而言，以下是一些重要示例：  
  
- XML 编写器通常不允许 XML 文档声明 (例如， \<?xml version=’1.0’ ?> 在编写另一个文档的过程中) 。 您不能使用完整的 XML 文档并将其作为 `Array` 数据成员的 `XmlNode` 进行序列化。 要执行此操作，您必须提取出文档声明或使用自己的编码方案表示它。  
  
- 使用 WCF 拒绝 XML 处理指令提供的所有 XML 编写器 (\<? … ?>) 和文档类型定义 (\<! … >) ，因为 SOAP 消息中不允许使用它们。 同样，您可以使用自己的编码机制避免此限制。 如果您必须要在生成的 XML 中包含这些内容，则可以编写一个自定义编码器，以使用支持这些内容的 XML 编写器。  
  
- 在实现 `WriteXml` 时，避免对 XML 编写器调用 <xref:System.Xml.XmlWriter.WriteRaw%2A> 方法。 WCF 使用各种 XML 编码 (包括二进制) ，因此很难或不可能使用 `WriteRaw` 此类结果，从而可在任何编码中使用结果。  
  
- 实现时 `WriteXml` ，请避免使用 <xref:System.Xml.XmlWriter.WriteEntityRef%2A> <xref:System.Xml.XmlWriter.WriteNmToken%2A> WCF 附带的 XML 编写器不支持的和方法。  
  
## <a name="using-dataset-typed-dataset-and-datatable"></a>使用数据集、类型化数据集和数据表  

 数据协定模型中完全支持使用这些类型。 使用这些类型时，请考虑以下事项：  
  
- 这些类型的架构 (尤其是 <xref:System.Data.DataSet> 其类型化的派生类) 可能无法与某些非 WCF 平台互操作，也可能会在与这些平台一起使用时导致可用性差。 另外，使用 `DataSet` 类型可能会影响性能。 最后，这可能会增加将来升级应用程序版本的难度。 考虑使用显式定义的数据协定类型代替协定中的 `DataSet` 类型。  
  
- 导入 `DataSet` 或 `DataTable` 架构时，引用这些类型很重要。 使用 Svcutil.exe 命令行工具，可以通过将 System.Data.dll 程序集名称传递给开关来完成此操作 `/reference` 。 如果导入类型化数据集架构，则必须引用类型化数据集的类型。 在 Svcutil.exe 中，将类型化数据集的程序集的位置传递到 `/reference` 开关。 有关引用类型的详细信息，请参阅 [导入架构以生成类](importing-schema-to-generate-classes.md)。  
  
 对数据协定模型中类型化数据集的支持是有限的。 类型化数据集可以序列化和反序列化，并可导出其架构。 但是，数据协定架构导入无法从该架构生成新的类型化数据集类型，因为它只能重新使用现有的类型化数据集类型。 通过对 Svcutil.exe 使用 `/r` 开关，可以指向现有类型化数据集。 如果尝试在不使用 `/r` 开关的情况下对使用类型化数据集的服务使用 Svcutil.exe，则会自动选择替代序列化程序 (XmlSerializer)。 如果必须使用 DataContractSerializer 且必须从架构生成数据集，则可使用以下过程：生成类型化数据集类型（通过将 Xsd.exe 工具与 `/d` 开关结合起来用于服务）、编译类型，然后在 Svcutil.exe 上使用 `/r` 开关来指向这些类型。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Xml.Serialization.IXmlSerializable>
- [使用数据协定](using-data-contracts.md)
- [数据协定序列化程序支持的类型](types-supported-by-the-data-contract-serializer.md)
