---
title: 导入架构以生成类
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, schema import and export
- XsdDataContractImporter class
ms.assetid: b9170583-8c34-43bd-97bb-6c0c8dddeee0
ms.openlocfilehash: 7f7c13cda2dcaeec16ad1ee49ff3dddf369906e5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280516"
---
# <a name="importing-schema-to-generate-classes"></a>导入架构以生成类

若要从与 Windows Communication Foundation (WCF) 一起使用的架构生成类，请使用 <xref:System.Runtime.Serialization.XsdDataContractImporter> 类。 本主题描述该过程和变体。  
  
## <a name="the-import-process"></a>导入过程

 架构导入过程以一个 <xref:System.Xml.Schema.XmlSchemaSet> 开始并生成一个 <xref:System.CodeDom.CodeCompileUnit>。  
  
 `XmlSchemaSet`是 .NET Framework 的架构对象模型的一部分 (SOM) ，它表示 (XSD) 架构文档的一组 XML 架构定义语言。 若要从 XSD 文档集创建一个 `XmlSchemaSet` 对象，则将每个文档反序列化到 <xref:System.Xml.Schema.XmlSchema> 对象中（使用 <xref:System.Xml.Serialization.XmlSerializer>），并将这些对象添加到新的 `XmlSchemaSet` 中。  
  
 `CodeCompileUnit`是 .NET Framework 代码文档对象模型的一部分 (CodeDOM) ，它以抽象方式表示 .NET Framework 代码。 若要从 `CodeCompileUnit` 生成实际代码，则使用 <xref:System.CodeDom.Compiler.CodeDomProvider> 类的一个子类，例如 <xref:Microsoft.CSharp.CSharpCodeProvider> 或 <xref:Microsoft.VisualBasic.VBCodeProvider> 类。  
  
### <a name="to-import-a-schema"></a>导入架构  
  
1. 创建 <xref:System.Runtime.Serialization.XsdDataContractImporter> 的实例：  
  
2. 可选。 在构造函数中传递一个 `CodeCompileUnit`。 将在架构导入期间生成的类型添加到此 `CodeCompileUnit` 实例中，而不是以空白 `CodeCompileUnit` 开始。  
  
3. 可选。 调用 <xref:System.Runtime.Serialization.XsdDataContractImporter.CanImport%2A> 方法之一。 该方法确定给定架构是否为有效的数据协定架构，以及是否可以被导入。 `CanImport` 方法具有与下一步中的 `Import` 相同的重载。  
  
4. 调用重载的 `Import` 方法之一，例如 <xref:System.Runtime.Serialization.XsdDataContractImporter.Import%28System.Xml.Schema.XmlSchemaSet%29> 方法。  
  
     最简单的重载采用 `XmlSchemaSet` 并导入在架构集中找到的所有类型，包括匿名类型。 使用其他重载可以指定 XSD 类型或要导入的类型列表（以 <xref:System.Xml.XmlQualifiedName> 或 `XmlQualifiedName` 对象集合的形式）。 在此情况下，仅导入指定的类型。 存在一个重载，它采用 <xref:System.Xml.Schema.XmlSchemaElement> 导入 `XmlSchemaSet` 之外的特定元素，以及它的关联类型（无论类型是否为匿名）。 该重载返回一个 `XmlQualifiedName`，表示为此元素生成的类型的数据协定名称。  
  
     多次调用 `Import` 方法会向同一个 `CodeCompileUnit` 添加多个项。 如果某个类型已经存在于 `CodeCompileUnit` 中，则不会再在其中生成该类型。 在同一个 `Import` 上多次调用 `XsdDataContractImporter`，而不是使用多个 `XsdDataContractImporter` 对象。 建议使用这种方法以避免生成重复的类型。  
  
    > [!NOTE]
    > 如果导入期间出现了故障，则 `CodeCompileUnit` 将处于不可预知的状态。 使用从失败的导入中产生的 `CodeCompileUnit` 可能会使您暴露出安全漏洞。  
  
5. 通过 `CodeCompileUnit` 属性访问 <xref:System.Runtime.Serialization.XsdDataContractImporter.CodeCompileUnit%2A> 。  
  
### <a name="import-options-customizing-the-generated-types"></a>导入选项：自定义生成的类型  

 可以将 <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> 的 <xref:System.Runtime.Serialization.XsdDataContractImporter> 属性设置为 <xref:System.Runtime.Serialization.ImportOptions> 类的一个实例，以便控制导入过程的各个方面。 许多选项会直接影响生成的类型。  
  
#### <a name="controlling-the-access-level-generateinternal-or-the-internal-switch"></a>控制访问级别（GenerateInternal 或 /internal 开关）  

 这对应于 " **/internal** " [元数据实用工具工具上的开关 ( # A0)](../servicemodel-metadata-utility-tool-svcutil-exe.md)。  
  
 通常情况下，公共类型是从架构生成的，具有私有字段和匹配的公共数据成员属性。 若要生成内部类型，则将 <xref:System.Runtime.Serialization.ImportOptions.GenerateInternal%2A> 属性设置为 `true`。  
  
 下面的示例演示在将 <xref:System.Runtime.Serialization.ImportOptions.GenerateInternal%2A> 属性设置为 `true.` 时转换为内部类的架构。  
  
 [!code-csharp[c_SchemaImportExport#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#2)]
 [!code-vb[c_SchemaImportExport#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#2)]  
  
#### <a name="controlling-namespaces-namespaces-or-the-namespace-switch"></a>控制命名空间（Namespaces 或 /namespace 开关）  

 这对应于工具中的 **/namespace** 开关 `Svcutil.exe` 。  
  
 通常，从架构生成的类型将生成到 .NET Framework 命名空间中，每个 XSD 命名空间对应于特定 .NET Framework 命名空间，具体取决于 [数据协定架构引用](data-contract-schema-reference.md)中所述的映射。 通过将 <xref:System.Runtime.Serialization.ImportOptions.Namespaces%2A> 属性设置为 <xref:System.Collections.Generic.Dictionary%602> 可以自定义此映射。 如果在字典中找到给定的 XSD 命名空间，则也将从字典中获取匹配的 .NET Framework 命名空间。  
  
 例如，考虑下面的架构。  
  
 [!code-xml[c_SchemaImportExport#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#10)]  
  
 下面的示例使用 `Namespaces` 属性将 `http://schemas.contoso.com/carSchema` 命名空间映射到 "Contoso"。  
  
 [!code-csharp[c_SchemaImportExport#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#8)]
 [!code-vb[c_SchemaImportExport#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#8)]  
  
#### <a name="adding-the-serializableattribute-generateserializable-or-the-serializable-switch"></a>添加 SerializableAttribute（GenerateSerializable 或 /serializable 开关）  

 这对应于工具中的 **/serializable** 开关 `Svcutil.exe` 。  
  
 有时，从架构生成的类型在 .NET Framework 运行时序列化引擎中可用 (例如， <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=nameWithType> 和类) ，这一点很重要 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 。 使用类型进行 .NET Framework 远程处理时，这非常有用。 若要启用它，则除常规 <xref:System.SerializableAttribute> 属性除外，还必须将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于生成的类型。 如果 `GenerateSerializable` 导入选项设置为 `true`，则将自动生成该属性。  
  
 下面的示例演示在 `Vehicle` 导入选项设置为 `GenerateSerializable` 时生成的 `true` 类。  
  
 [!code-csharp[c_SchemaImportExport#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#4)]
 [!code-vb[c_SchemaImportExport#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#4)]  
  
#### <a name="adding-data-binding-support-enabledatabinding-or-the-enabledatabinding-switch"></a>添加数据绑定支持（EnableDataBinding 或 /enableDataBinding 开关）  

 这对应于 Svcutil.exe 工具上的 **/enableDataBinding** 开关。  
  
 有时，可能需要将从架构生成的类型绑定到图形用户界面组件，这样这些类型实例的任何更新都将自动更新 UI。 `XsdDataContractImporter` 可以生成实现 <xref:System.ComponentModel.INotifyPropertyChanged> 接口的类型，以使任何属性更改都触发事件。 如果要生成的类型适用于支持此接口的客户端 UI 编程环境 (例如 Windows Presentation Foundation (WPF) # A3，请将属性设置 <xref:System.Runtime.Serialization.ImportOptions.EnableDataBinding%2A> 为 `true` 以启用此功能。  
  
 下面的示例演示在 `Vehicle` 设置为 <xref:System.Runtime.Serialization.ImportOptions.EnableDataBinding%2A> 时生成的 `true` 类。  
  
 [!code-csharp[C_SchemaImportExport#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#5)]
 [!code-vb[C_SchemaImportExport#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#5)]  
  
### <a name="import-options-choosing-collection-types"></a>导入选项：选择集合类型  

 XML 中存在两种特殊的模式，用于表示项集合：项列表和项与项之间的关联。 下面是一个字符串列表示例。  
  
 [!code-xml[C_SchemaImportExport#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#11)]  
  
 下面是一个字符串与整数（`city name` 和 `population`）之间的关联示例。  
  
 [!code-xml[C_SchemaImportExport#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#12)]  
  
> [!NOTE]
> 也可将任何关联视为列表。 例如，可以将上面的关联视为一个复杂的 `city` 对象列表，这些对象碰巧有两个字段（一个字符串字段和一个整数字段）。 这两种模式在 XSD 架构中都有一种表示形式。 没有办法区分列表和关联，因此此类模式始终被视为列表，除非架构中存在特定于 WCF 的特殊批注。 该批注指示给定模式表示一个关联。 有关详细信息，请参阅 [数据协定架构参考](data-contract-schema-reference.md)。  
  
 通常情况下，列表将作为派生自泛型列表或 .NET Framework 数组的集合数据协定导入，具体取决于架构是否遵循集合的标准命名模式。 [数据协定中的集合类型](collection-types-in-data-contracts.md)更详细地介绍了这种情况。 通常将关联作为 <xref:System.Collections.Generic.Dictionary%602> 或派生自字典对象的集合数据协定导入。 例如，考虑下面的架构。  
  
 [!code-xml[c_SchemaImportExport#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#13)]  
  
 这将按照下面的方式导入（出于可读性目的，显示的是字段而非属性）。  
  
 [!code-csharp[c_SchemaImportExport#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#6)]
 [!code-vb[c_SchemaImportExport#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#6)]  
  
 可以自定义为此类架构模式生成的集合类型。 例如，可能需要生成派生自 <xref:System.ComponentModel.BindingList%601> 而非 <xref:System.Collections.Generic.List%601> 类的集合，以将此类型绑定到列表框并在集合内容更改时使其自动更新。 为此，将 <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> 类的 <xref:System.Runtime.Serialization.ImportOptions> 属性设置为要使用的集合类型（以后称为引用的类型）的列表。 导入任何集合时，将扫描引用的集合类型的列表，并使用最佳匹配集合（如果能够找到）。 关联只能与实现泛型或非泛型 <xref:System.Collections.IDictionary> 接口的类型相匹配，而列表可与任何受支持的集合类型相匹配。  
  
 例如，如果 <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> 属性设置为 <xref:System.ComponentModel.BindingList%601>，则按下面的方式生成上面示例中的 `people` 类型。  
  
 [!code-csharp[C_SchemaImportExport#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#7)]
 [!code-vb[C_SchemaImportExport#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#7)]  
  
 将封闭式泛型视为最佳匹配。 例如，如果将类型 `BindingList(Of Integer)` 和 <xref:System.Collections.ArrayList> 传递给引用的类型集合，则将在架构中找到的任何整数列表作为 `BindingList(Of Integer)` 导入。 将任何其他列表（例如 `List(Of String)`）作为 `ArrayList` 导入。  
  
 如果将实现泛型 `IDictionary` 接口的一个类型添加到引用的类型集合，则其类型参数必须完全打开或完全封闭。  
  
 不允许重复。 例如，不能同时向引用的类型添加 `List(Of Integer)` 和 `Collection(Of Integer)`。 这会使在架构中找到整数列表时无法确定应该使用哪一个整数。 仅当架构中的某个类型暴露出重复问题时才能检测到重复项。 例如，如果导入的架构中不包含整数列表，则允许引用的类型集合中同时具有 `List(Of Integer)` 和 `Collection(Of Integer)`，两者都不会产生任何影响。  
  
 引用的集合类型机制不仅适用于基元集合，也适用于复杂类型集合（包括其他集合的集合）。  
  
 `ReferencedCollectionTypes`属性对应于 SvcUtil.exe 工具上的 **/collectionType** 开关。 请注意，若要引用多个集合类型，必须多次指定 **/collectionType** 开关。 如果该类型不在 MsCorLib.dll 中，则还必须使用 **/reference** 开关引用其程序集。  
  
#### <a name="import-options-referencing-existing-types"></a>导入选项：引用现有类型  

 有时，架构中的类型对应于现有 .NET Framework 类型，无需从头开始生成这些类型。 （本节仅适用于非集合类型。 有关集合类型，请参见上一节。）  
  
 例如，您可能有一个标准公司范围内的“Person”数据协定类型，通常在表示人员时需要使用它。 每当某个服务利用该类型且其架构显示在该服务元数据中时，在导入此架构时您可能需要重用现有的 `Person` 类型，而不需要为每个服务生成新的类型。  
  
 为此，请将你想要重用的 .NET Framework 类型的列表传递到 <xref:System.Runtime.Serialization.ImportOptions.ReferencedTypes%2A> 属性在类上返回的集合中 <xref:System.Runtime.Serialization.ImportOptions> 。 如果任何这些类型的数据协定名称和命名空间与架构类型的名称和命名空间相匹配，则执行结构比较。 如果确定这些类型同时具有匹配的名称和匹配结构，则将重用现有的 .NET Framework 类型，而不是生成新的类型。 如果仅名称匹配而构造不匹配，则引发异常。 请注意，引用类型（例如，添加新的可选数据成员）时不允许进行版本管理。 构造必须完全匹配。  
  
 向引用的类型集合添加多个具有相同数据协定名称和命名空间的类型是合法的，前提是不导入具有该名称和命名空间的架构类型。 这样您就可以轻松地将程序集中的所有类型添加到集合中，而无需担心在架构中并未实际出现的类型重复项。  
  
 `ReferencedTypes`属性对应于在 Svcutil.exe 工具的某些操作模式下的 **/reference** 开关。  
  
> [!NOTE]
> 在 Visual Studio 中使用 Svcutil.exe 或 () **添加服务引用** 工具时，将自动引用 MsCorLib.dll 中的所有类型。  
  
#### <a name="import-options-importing-non-datacontract-schema-as-ixmlserializable-types"></a>导入选项：将 Non-DataContract 架构作为 IXmlSerializable 类型导入  

 <xref:System.Runtime.Serialization.XsdDataContractImporter> 支持架构的有限子集。 如果出现不受支持的架构构造（例如，XML 属性），导入尝试会失败并引发异常。 但是，将 <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> 属性设置为 `true` 可扩展受支持架构的范围。 设置为 `true` 时，<xref:System.Runtime.Serialization.XsdDataContractImporter> 生成可实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口的类型。 这样可直接访问这些类型的 XML 表示形式。  
  
##### <a name="design-considerations"></a>设计注意事项  
  
- 直接使用弱类型 XML 表示形式可能有些困难。 可以考虑使用其他序列化引擎（例如 <xref:System.Xml.Serialization.XmlSerializer>），以便以强类型方式使用与数据协定不兼容的架构。 有关详细信息，请参阅 [使用 XmlSerializer 类](using-the-xmlserializer-class.md)。  
  
- 某些架构构造不能通过 <xref:System.Runtime.Serialization.XsdDataContractImporter> 事件导入，即使是 <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> 属性设置为 `true`。 此外，在此情况下可以考虑使用 <xref:System.Xml.Serialization.XmlSerializer>。  
  
- 当 <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> 是 `true` 或 `false` 在 [数据协定架构引用](data-contract-schema-reference.md)中描述时，支持的确切架构构造。  
  
- 生成的 <xref:System.Xml.Serialization.IXmlSerializable> 类型的架构在导入和导出时不保留保真。 也就是说，从生成的类型导出架构或作为类导入时不返回原始架构。  
  
 可以结合使用 <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A> 选项和上述 <xref:System.ServiceModel.Description.ServiceContractGenerator.ReferencedTypes%2A> 选项。 对于那些必须作为 <xref:System.Xml.Serialization.IXmlSerializable> 实现生成的类型，使用 <xref:System.ServiceModel.Description.ServiceContractGenerator.ReferencedTypes%2A> 功能时将跳过结构检查。  
  
 <xref:System.Runtime.Serialization.ImportOptions.ImportXmlType%2A>选项对应于 Svcutil.exe 工具上的 **/importXmlTypes** 开关。  
  
##### <a name="working-with-generated-ixmlserializable-types"></a>使用生成的 IXmlSerializable 类型  

 生成的 `IXmlSerializable` 类型包含名为“nodesField”的私有字段，该字段返回一个 <xref:System.Xml.XmlNode> 对象数组。 反序列化此类的实例时，可以使用 XML 文档对象模型直接通过该字段访问 XML 数据。 序列化此类的实例时，可以将该字段设置为所需的 XML 数据，然后它将被序列化。  
  
 这是通过 `IXmlSerializable` 实现完成的。 在生成的 `IXmlSerializable` 类型中，<xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> 实现调用 <xref:System.Runtime.Serialization.XmlSerializableServices.ReadNodes%2A> 类的 <xref:System.Runtime.Serialization.XmlSerializableServices> 方法。 该方法是一个帮助器方法，将所提供的 XML 通过 <xref:System.Xml.XmlReader> 转换为一个 <xref:System.Xml.XmlNode> 对象数组。 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 实现执行相反操作，将 `XmlNode` 对象数组转换为一个 <xref:System.Xml.XmlWriter> 调用序列。 这是使用 <xref:System.Runtime.Serialization.XmlSerializableServices.WriteNodes%2A> 方法完成的。  
  
 可以在生成的 `IXmlSerializable` 类上运行架构导出过程。 如上所述，您将不会重新获得原始架构。 相反，你将获得 "anyType" 标准 XSD 类型，它是任何 XSD 类型的通配符。  
  
 这是通过将属性应用 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 到生成的 `IXmlSerializable` 类并指定调用 <xref:System.Runtime.Serialization.XmlSerializableServices.AddDefaultSchema%2A> 方法以生成 "anyType" 类型的方法来实现的。  
  
> [!NOTE]
> <xref:System.Runtime.Serialization.XmlSerializableServices> 类型存在的目的只是为了支持此特定功能。 建议不将其用于任何其他目的。  
  
#### <a name="import-options-advanced-options"></a>导入选项：高级选项  

 下面介绍高级导入选项：  
  
- <xref:System.Runtime.Serialization.ImportOptions.CodeProvider%2A> 属性。 指定用于为生成的类生成代码的 <xref:System.CodeDom.Compiler.CodeDomProvider>。 导入机制尝试避免不受 <xref:System.CodeDom.Compiler.CodeDomProvider> 支持的功能。 如果 <xref:System.Runtime.Serialization.ImportOptions.CodeProvider%2A> 未设置，则使用一组完整的 .NET Framework 功能，无任何限制。  
  
- <xref:System.Runtime.Serialization.ImportOptions.DataContractSurrogate%2A> 属性。 一个可用此属性指定的 <xref:System.Runtime.Serialization.IDataContractSurrogate> 实现。 <xref:System.Runtime.Serialization.IDataContractSurrogate> 自定义导入过程。 有关详细信息，请参阅 [数据协定代理](../extending/data-contract-surrogates.md)项。 默认情况下，不使用代理项。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- <xref:System.Runtime.Serialization.ImportOptions>
- [数据协定架构参考](data-contract-schema-reference.md)
- [数据协定代理项](../extending/data-contract-surrogates.md)
- [架构导入和导出](schema-import-and-export.md)
- [从类导出架构](exporting-schemas-from-classes.md)
- [数据协定架构参考](data-contract-schema-reference.md)
