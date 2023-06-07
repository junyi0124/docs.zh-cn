---
title: 数据协定中的集合类型
description: 了解数据协定模型如何处理 .NET Framework 中的集合，以及 WCF 如何支持集合类型的数据序列化。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- collection types [WCF], data contracts
- data contracts [WCF], collection types
- collection types [WCF]
ms.assetid: 9b45b28e-0a82-4ea3-8c33-ec0094aff9d5
ms.openlocfilehash: 83acf1f74bf3cb117f3f94743eda32d3f2cc4b82
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245175"
---
# <a name="collection-types-in-data-contracts"></a>数据协定中的集合类型

** “集合”指特定类型的项的列表。 在 .NET Framework 中，可以使用数组或其他各种类型（泛型列表、泛型 <xref:System.ComponentModel.BindingList%601> 、或）来表示此类列表 <xref:System.Collections.Specialized.StringCollection> <xref:System.Collections.ArrayList> 。 例如，集合可以容纳给定客户的地址列表。 无论这些集合的实际类型是什么，这些集合都称为“列表集合” **。

存在一种特殊形式的集合，该集合表示某一项（“键”）与另一项（“值”）之间的关联。 在 .NET Framework 中，它们由类型（如） <xref:System.Collections.Hashtable> 和泛型字典来表示。 例如，一个关联集合可能将城市（“键”）映射到它的人口数量（“值”）。 无论这些集合的实际类型是什么，这些集合都称为“字典集合” **。

集合在数据协定模型中受到特殊对待。

实现 <xref:System.Collections.IEnumerable> 接口的类型（包括数组和泛型集合）被识别为集合。 其中，实现 <xref:System.Collections.IDictionary> 或泛型 <xref:System.Collections.Generic.IDictionary%602> 接口的类型是字典集合，其他所有类型是列表集合。

以下各节将详细讨论集合类型的其他要求（例如，具有一个名为的方法 `Add` 和一个无参数的构造函数）。 这确保了既可以对集合类型序列化，也可以对其反序列化。 这意味着不直接支持某些集合，例如泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> （因为它没有无参数的构造函数）。 但是，也可以避开这些限制。有关信息，请参见本主题后面的“使用集合接口类型和只读集合”。

包含在集合中的类型必须是数据协定类型，或者是可序列化的。 有关详细信息，请参阅[数据协定序列化程序支持的类型](types-supported-by-the-data-contract-serializer.md)。

有关什么是以及哪些内容不被视为有效集合，以及如何序列化集合的详细信息，请参阅本主题的 "高级集合规则" 部分中有关序列化集合的信息。

## <a name="interchangeable-collections"></a>可互换的集合

同一类型的所有列表集合均被视为具有相同的数据协定（除非它们是使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性自定义的，如本主题后面所述）。 因此，举例而言，下面的数据协定是等效的。

[!code-csharp[c_collection_types_in_data_contracts#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#0)]
[!code-vb[c_collection_types_in_data_contracts#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#0)]

两个数据协定均产生类似于以下代码的 XML。

```xml
<PurchaseOrder>
    <customerName>...</customerName>
    <items>
        <Item>...</Item>
        <Item>...</Item>
        <Item>...</Item>
        ...
    </items>
    <comments>
        <string>...</string>
        <string>...</string>
        <string>...</string>
        ...
    </comments>
</PurchaseOrder>
```

举例而言，集合的可互换性使您可以使用一个针对服务器性能而优化的集合类型，以及一个旨在绑定到客户端上的用户界面组件的集合类型。

与列表集合类似，具有相同键和值类型的所有字典集合都被视为具有相同的数据协定（除非通过 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性自定义）。

就集合等效性而言，只有数据协定类型才有意义，.NET 类型并无意义。 也就是说，如果 Type1 和 Type2 具有等效的数据协定，那么 Type1 的集合将被视为与 Type2 的集合等效。

非泛型集合被视为与 `Object`类型的泛型集合具有相同的数据协定。 （例如， <xref:System.Collections.ArrayList> 的数据协定与 <xref:System.Collections.Generic.List%601> 的泛型 `Object` 是相同的。）

## <a name="using-collection-interface-types-and-read-only-collections"></a>使用集合接口类型和只读集合

集合接口类型（<xref:System.Collections.IEnumerable>、 <xref:System.Collections.IDictionary>、泛型 <xref:System.Collections.Generic.IDictionary%602>或从这些接口派生的接口）也被视为具有与实际集合类型的集合数据协定等效的集合数据协定。 因此，可以声明被序列化为集合接口类型的类型，并且结果与使用实际集合类型时的结果相同。 例如，下面的数据协定是等效的。

[!code-csharp[c_collection_types_in_data_contracts#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#1)]
[!code-vb[c_collection_types_in_data_contracts#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#1)]

序列化期间，当声明的类型是接口时，使用的实际实例类型可以是实现该接口的任何类型。 前面讨论的限制（具有无参数的构造函数和 `Add` 方法）不适用。 例如，即使您不能直接声明泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 类型的数据成员，仍然可以将 Customer2 中的地址设置为 Address 的泛型 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601>的实例。

反序列化期间，当声明的类型是接口时，序列化引擎会选择实现所声明的接口的类型，并且该类型会实例化。 已知的类型机制（在[数据协定已知类型](data-contract-known-types.md)中介绍）在这里没有任何效果;类型的选择内置于 WCF 中。

## <a name="customizing-collection-types"></a>自定义集合类型

您可以通过使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性来自定义集合类型，该属性具有几种用法。

请注意，自定义集合类型会危害集合的可互换性，因此通常建议尽量避免应用此特性。 有关此问题的详细信息，请参阅本主题后面的 "高级集合规则" 一节。

### <a name="collection-data-contract-naming"></a>集合数据协定命名

如 [Data Contract Names](data-contract-names.md)中所述，命名集合类型的规则与命名常规数据协定类型的规则类似，但存在一些重要的区别：

- 使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性（而不是 <xref:System.Runtime.Serialization.DataContractAttribute> 属性）来自定义名称。 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性 (Attribute) 也有 `Name` 和 `Namespace` 属性 (Property)。

- 当未应用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性时，集合类型的默认名称和命名空间依赖于集合内包含的类型的名称和命名空间。 它们不受集合类型本身的名称和命名空间的影响。 有关示例，请参见以下类型。

  ```csharp
  public CustomerList1 : Collection<string> {}
  public StringList1 : Collection<string> {}
  ```

两个类型的数据协定名称都是“ArrayOfstring”，而不是“CustomerList1”或“StringList1”。 这意味着在根级别序列化其中任何一个类型都将产生与下面的代码类似的 XML。

```xml
<ArrayOfstring>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</ArrayOfstring>
```

选择此命名规则可确保表示字符串列表的任何非自定义类型都具有相同的数据协定和 XML 表示。 这使得集合的可互换性成为可能。 在本例中，CustomerList1 和 StringList1 是完全可互换的。

但是，当应用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性时，集合将成为一个自定义的集合数据协定，即使未在该属性 (Attribute) 上设置任何属性 (Property) 也将如此。 之后，集合数据协定的名称和命名空间将取决于集合类型本身。 有关示例，请参见下面的类型。

[!code-csharp[c_collection_types_in_data_contracts#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#2)]
[!code-vb[c_collection_types_in_data_contracts#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#2)]

序列化后，所产生的 XML 将类似于以下代码：

```xml
<CustomerList2>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</CustomerList2>
```

请注意，这不再等效于非自定义类型的 XML 表示。

- 您可以使用 `Name` 和 `Namespace` 属性来进一步自定义命名。 请参见下面的类。

  [!code-csharp[c_collection_types_in_data_contracts#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#3)]
  [!code-vb[c_collection_types_in_data_contracts#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#3)]

所产生的 XML 类似于以下内容。

```xml
<cust_list>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</cust_list>
```

有关详细信息，请参阅本主题后面的 "高级集合规则" 一节。

### <a name="customizing-the-repeating-element-name-in-list-collections"></a>自定义列表集合中的重复元素名称

列表集合包含重复项。 通常，每个重复项都表示为根据集合中包含的类型的数据协定名称而命名的元素。

在 `CustomerList` 示例中，集合包含字符串。 String 基元类型的数据协定名称为 "string"，因此重复元素为 " \<string> "。

但是，通过对 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> 属性 (Attribute) 使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性 (Property)，可以自定义该重复元素名称。 有关示例，请参见下面的类型。

[!code-csharp[c_collection_types_in_data_contracts#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#4)]
[!code-vb[c_collection_types_in_data_contracts#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#4)]

所产生的 XML 类似于以下内容。

```xml
<CustomerList4>
    <customer>...</customer>
    <customer>...</customer>
    <customer>...</customer>
    ...
</CustomerList4>
```

重复元素的命名空间始终与集合数据协定的命名空间相同，如前所述，该命名空间可以使用 `Namespace` 属性自定义。

### <a name="customizing-dictionary-collections"></a>自定义字典集合

字典集合实质上是项列表，其中每一项都有一个后面跟随值的键。 与常规列表一样，您可以使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> 属性更改与重复元素对应的元素名称。

此外，您还可以使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> 和 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> 属性来更改表示键和值的元素名称。 这些元素的命名空间与集合数据协定的命名空间相同。

有关示例，请参见下面的类型。

[!code-csharp[c_collection_types_in_data_contracts#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#5)]
[!code-vb[c_collection_types_in_data_contracts#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#5)]

序列化后，所产生的 XML 将类似于以下代码：

```xml
<CountriesOrRegionsWithCapitals>
    <entry>
        <countryorregion>USA</countryorregion>
        <capital>Washington</capital>
    </entry>
    <entry>
        <countryorregion>France</countryorregion>
        <capital>Paris</capital>
    </entry>
    ...
</CountriesOrRegionsWithCapitals>
```

有关字典集合的详细信息，请参阅本主题后面的 "高级集合规则" 一节。

## <a name="collections-and-known-types"></a>集合和已知类型

如果集合类型是以多态方式来代替其他集合或集合接口的，您不需要将这样的集合类型添加到已知类型中。 例如，如果您声明一个 <xref:System.Collections.IEnumerable> 类型的数据成员并将其用于发送 <xref:System.Collections.ArrayList>的一个实例，则无需将 <xref:System.Collections.ArrayList> 添加到已知类型。

当您以多态方式使用集合来代替非集合类型时，必须将这些集合添加到已知类型。 例如，如果您声明一个 `Object` 类型的数据成员并将其用于发送 <xref:System.Collections.ArrayList>的一个实例，则需要将 <xref:System.Collections.ArrayList> 添加到已知类型中。

这不允许您以多态方式序列化任何等效集合。 例如，当您将 <xref:System.Collections.ArrayList> 添加到前面示例中的已知类型列表时，这不允许您指定 `Array of Object` 类，即使该类具有等效的数据协定也是如此。 这与常规的已知类型在非集合类型的序列化方面的行为没有任何不同，但对于集合而言了解这一点尤为重要，因为集合的等效性是很常见的。

在序列化期间，在给定数据协定的任何给定范围内，只能有一个类型是已知的，并且等效集合都有相同的数据协定。 这意味着，在前面的示例中，您不能将 <xref:System.Collections.ArrayList> 和 `Array of Object` 都添加到同一范围中的已知类型。 同样，这与已知类型在非集合类型方面的行为是等效的，但对于集合而言，了解这一点尤为重要。

对于集合的内容而言，已知类型可能也是必需的。 例如，如果 <xref:System.Collections.ArrayList> 实际上包含 `Type1` 和 `Type2`的实例，则应将这两个类型都添加到已知类型中。

下面的示例演示使用集合和已知类型正确构造的对象图 本示例虚构成分稍浓，因为在实际的应用程序中，通常不会将下面的数据成员定义为 `Object`，因此不会有任何已知类型/多态性问题。

[!code-csharp[c_collection_types_in_data_contracts#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#6)]
[!code-vb[c_collection_types_in_data_contracts#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#6)]

在反序列化时，如果声明的类型是集合类型，则无论实际发送的是什么类型，都会实例化所声明的类型。 如果所声明的类型是集合接口，则反序列化程序会选取一个要实例化的类型，而不会考虑该类型是否为已知类型。

此外，在反序列化时，如果声明的类型不是一个集合类型，但要发送的是集合类型，则会在已知类型列表之中选取一个匹配的集合类型。 可以在反序列化时将集合接口类型添加到已知类型列表中。 在这种情况下，反序列化引擎会再次选取要实例化的类型。

## <a name="collections-and-the-netdatacontractserializer-class"></a>集合和 NetDataContractSerializer 类

当正在使用 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类时，非数组的非自定义集合类型（无 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性）会失去其特殊意义。

用 <xref:System.SerializableAttribute> 属性标记的非自定义集合类型仍然可以由 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类根据 <xref:System.SerializableAttribute> 属性或 <xref:System.Runtime.Serialization.ISerializable> 接口规则来序列化。

自定义的集合类型、集合接口以及数组仍然被视为集合，即使当正在使用 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类时也将如此。

## <a name="collections-and-schema"></a>集合与架构

所有等效的集合在 XML 架构定义 (XSD) 语言架构中都具有相同的表示。 因此，您通常不会在所生成的客户端代码中获得与服务器上的集合类型相同的集合类型。 例如，服务器可能使用具有 Integer 数据成员的泛型 <xref:System.Collections.Generic.List%601> 的数据协定，但是在生成的客户端代码中，该数据成员可能成为整数数组。

字典集合标记有特定于 WCF 的架构批注，该批注指示它们是字典;否则，它们不能与包含具有键和值的项的简单列表区分开来。 有关如何在数据协定架构中表示集合的准确说明，请参阅 [Data Contract Schema Reference](data-contract-schema-reference.md)。

默认情况下，不会为导入的代码中的非自定义集合生成类型。 列表集合类型的数据成员是作为数组导入的，字典集合类型的数据成员是作为泛型字典导入的。

但是，对于自定义集合，将生成单独的类型，这样的类型会标记有 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性。 （架构中的自定义集合类型不使用默认命名空间、名称、重复元素名称或键/值元素名称。）这些类型是为列表类型派生的空类型 <xref:System.Collections.Generic.List%601> 和字典类型的泛型字典。

例如，您可能在服务器上有以下类型。

[!code-csharp[c_collection_types_in_data_contracts#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#7)]
[!code-vb[c_collection_types_in_data_contracts#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#7)]

当导出并再次导入架构时，生成的客户端代码与下面类似（为便于阅读，显示的是字段而不是属性）。

[!code-csharp[c_collection_types_in_data_contracts#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#8)]
[!code-vb[c_collection_types_in_data_contracts#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#8)]

您可能想要在生成的代码中使用与默认类型不同的类型。 例如，为了使数据成员易于绑定到用户界面组件，您可能想对它们使用泛型 <xref:System.ComponentModel.BindingList%601> ，而不使用常规数组。

若要选择要生成的集合类型，请在导入架构时，将要使用的集合类型的列表传递到 <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> 对象的 <xref:System.Runtime.Serialization.ImportOptions> 属性。 这些类型称为“引用的集合类型” **。

当引用泛型类型时，这些类型必须要么是完全开放式泛型，要么是完全封闭式泛型。

> [!NOTE]
> 当使用 Svcutil.exe 工具时，可以使用 **/collectionType** 命令行开关（简写形式是 **/ct**）来完成此引用。 请记住，还必须使用 **/reference** 开关（简写形式是 **/r**）指定引用的集合类型的程序集。 如果此类型是泛型，则它后面必须跟有反引号和泛型参数的数目。 后面的引号（ \` ）不会与单引号（'）字符混淆。 你可以通过多次使用 **/collectionType** 开关来指定多个引用的集合类型。

例如，使所有列表作为泛型 <xref:System.Collections.Generic.List%601>导入。

```console
svcutil.exe MyService.wsdl MyServiceSchema.xsd /r:C:\full_path_to_system_dll\System.dll /ct:System.Collections.Generic.List`1
```

导入任意集合时，都会扫描这一引用的集合类型列表，如果找到一个最佳匹配集合，则会将该集合用作数据成员类型（对于非自定义集合）或可从中派生其他类型的基类型（对于自定义集合）。 字典只能与字典匹配，列表只能与列表匹配。

例如，如果您将泛型 <xref:System.ComponentModel.BindingList%601> 和 <xref:System.Collections.Hashtable> 添加到引用类型列表，则前面示例的生成客户端代码将类似于以下代码：

[!code-csharp[c_collection_types_in_data_contracts#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#9)]
[!code-vb[c_collection_types_in_data_contracts#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#9)]

您可以将集合接口类型指定为引用的集合类型的一部分，但不能指定无效的集合类型（例如，没有 `Add` 方法或公共构造函数的类型）。

封闭式泛型被视为最佳匹配 （非泛型类型被视为与 `Object`的封闭式泛型等效）。 例如，如果 <xref:System.Collections.Generic.List%601> 的泛型 <xref:System.DateTime>、泛型 <xref:System.ComponentModel.BindingList%601> （开放式泛型）和 <xref:System.Collections.ArrayList> 类型是引用的集合类型，则会生成以下代码：

[!code-csharp[c_collection_types_in_data_contracts#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#10)]
[!code-vb[c_collection_types_in_data_contracts#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#10)]

对于列表集合，只支持下面的表中的情况。

|引用类型|引用类型所实现的接口|示例|类型被视为：|
|---------------------|----------------------------------------------|-------------|----------------------|
|非泛型或封闭式泛型（任意多个参数）|非泛型|`MyType : IList`<br /><br /> 或<br /><br /> `MyType<T> : IList`<br /><br /> 其中 T= `int`|`Object` 的封闭式泛型（例如， `IList<object>`）|
|非泛型或封闭式泛型（可以有任意多个参数，而且这些参数不是必须与集合类型匹配）|封闭式泛型|`MyType : IList<string>`<br /><br /> 或<br /><br /> `MyType<T> : IList<string>` 其中 T=`int`|封闭式泛型（例如 `IList<string>`）|
|可以有任意多个参数的封闭式泛型|使用该类型的任何一个参数的开放式泛型|`MyType<T,U,V> : IList<U>`<br /><br /> 其中 T=`int`，U=`string`，V=`bool`|封闭式泛型（例如 `IList<string>`）|
|具有一个参数的开放式泛型|使用该类型的参数的开放式泛型|`MyType<T> : IList<T>`，T 是开放式的|开放式泛型（例如 `IList<T>`）|

如果类型实现多个列表集合接口，则下列限制适用：

- 如果类型针对不同的类型多次实现泛型 <xref:System.Collections.Generic.IEnumerable%601> （或它的派生接口），则该类型不会被视为有效的引用集合类型，因此会被忽略。 即使有些实现是无效的或者使用开放式泛型，也将如此。 例如，实现 <xref:System.Collections.Generic.IEnumerable%601> 的泛型 `int` 以及 T 的泛型 <xref:System.Collections.Generic.IEnumerable%601> 的类型绝不会用作 `int` 或其他任何类型的引用集合，无论该类型是否具有接受 `Add` 的 `int` 方法和/或接受类型 T 的参数的 `Add` 方法，都将如此。

- 如果该类型实现一个泛型集合接口和 <xref:System.Collections.IList>，则该类型将绝不会用作引用的集合类型，除非该泛型集合接口是 <xref:System.Object>类型的封闭式泛型。

对于字典集合，只支持下面的表中的情况。

|引用类型|引用类型所实现的接口|示例|类型被视为|
|---------------------|----------------------------------------------|-------------|---------------------|
|非泛型或封闭式泛型（任意多个参数）|<xref:System.Collections.IDictionary>|`MyType : IDictionary`<br /><br /> 或<br /><br /> `MyType<T> : IDictionary` 其中 T=`int`|封闭式泛型 `IDictionary<object,object>`|
|封闭式泛型（任意多个参数）|<xref:System.Collections.Generic.IDictionary%602>，封闭式|`MyType<T> : IDictionary<string, bool>` 其中 T=`int`|封闭式泛型（例如 `IDIctionary<string,bool>`）|
|封闭式泛型（任意多个参数）|泛型 <xref:System.Collections.Generic.IDictionary%602>，键或值中的一个是封闭式的，另一个是开放式的，并使用类型的某个参数|`MyType<T,U,V> : IDictionary<string,V>` ，其中 T=`int`，U=`float`，V=`bool`<br /><br /> 或<br /><br /> `MyType<Z> : IDictionary<Z,bool>` ，其中 Z=`string`|封闭式泛型（例如 `IDictionary<string,bool>`）|
|封闭式泛型（任意多个参数）|泛型 <xref:System.Collections.Generic.IDictionary%602>，键和值均是开放式的，且每个都使用类型的一个参数|`MyType<T,U,V> : IDictionary<V,U>` 其中 T=`int`，U=`bool`，V=`string`|封闭式泛型（例如 `IDictionary<string,bool>`）|
|开放式泛型（两个参数）|开放式泛型 <xref:System.Collections.Generic.IDictionary%602>，按显示顺序使用类型的两个泛型参数|`MyType<K,V> : IDictionary<K,V>`，K 和 V 均是开放式的|开放式泛型（例如 `IDictionary<K,V>`）|

如果类型同时实现 <xref:System.Collections.IDictionary> 和泛型 <xref:System.Collections.Generic.IDictionary%602>，则只将考虑泛型 <xref:System.Collections.Generic.IDictionary%602> 。

不支持引用部分泛型类型。

不允许重复，例如，不能将 <xref:System.Collections.Generic.List%601> 的泛型 `Integer` 和 `Integer` 的泛型集合都添加到 <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A>中，因此这会使得当在架构中找到整数列表时无法确定使用其中的哪一个。 只有当架构中存在暴露重复问题的类型时，才会检测重复项。 例如，如果导入的架构不包含整数列表，则在 <xref:System.Collections.Generic.List%601> 中可以同时具有 `Integer` 的泛型 `Integer` 和 <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A>的泛型集合，但是两者都没有任何效果。

## <a name="advanced-collection-rules"></a>高级集合规则

### <a name="serializing-collections"></a>序列化集合

下面是集合序列化规则的列表：

- 允许组合集合类型（具有集合的集合）。 交错数组被视为集合的集合。 不支持多维数组。

- 字节数组和 <xref:System.Xml.XmlNode> 数组是特殊的数组类型，将被视为基元，而不是集合。 序列化字节数组会产生单个包含一个 Base64 编码数据块的 XML 元素，而不是为每个字节都生成一个单独的元素。 有关如何处理的数组的详细信息 <xref:System.Xml.XmlNode> ，请参阅[数据协定中的 XML 和 ADO.NET 类型](xml-and-ado-net-types-in-data-contracts.md)。 当然，这些特殊类型本身可以参与集合：字节数组的数组会产生多个 XML 元素，其中每个元素都包含一个 Base64 编码数据块。

- 如果 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于集合类型，则该类型会被视为常规数据协定类型，而不是集合。

- 如果集合类型实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口，下列规则适用（假定类型为 `myType:IList<string>, IXmlSerializable`）：

  - 如果声明类型为 `IList<string>`，则将类型序列化为列表。

  - 如果声明类型为 `myType`，则将该声明类型序列化为 `IXmlSerializable`。

  - 如果该声明类型为 `IXmlSerializable`，则该声明类型将序列化为 `IXmlSerializable`，但前提是将 `myType` 添加到已知类型的列表。

- 使用下表中显示的方法对集合进行序列化和反序列化。

|集合类型实现|序列化时调用的方法|反序列化时调用的方法|
|--------------------------------|-----------------------------------------|-------------------------------------------|
|泛型 <xref:System.Collections.Generic.IDictionary%602>|`get_Keys`, `get_Values`|泛型 Add|
|<xref:System.Collections.IDictionary>|`get_Keys`, `get_Values`|`Add`|
|泛型 <xref:System.Collections.Generic.IList%601>|泛型 <xref:System.Collections.Generic.IList%601> 索引器|泛型 Add|
|泛型 <xref:System.Collections.Generic.ICollection%601>|枚举器|泛型 Add|
|<xref:System.Collections.IList>|<xref:System.Collections.IList> 索引器|`Add`|
|泛型 <xref:System.Collections.Generic.IEnumerable%601>|`GetEnumerator`|一个称为 `Add` 的非静态方法，它采用一个相应类型（泛型参数的类型或者它的某个基类型）的参数。 必须存在这样的方法，序列化程序才能在序列化和反序列化期间，都将集合类型视为集合。|
|<xref:System.Collections.IEnumerable> （因此也包括派生自它的 <xref:System.Collections.ICollection>）|`GetEnumerator`|一个名为 `Add` 的非静态方法，它采用一个 `Object`类型的参数。 必须存在这样的方法，序列化程序才能在序列化和反序列化期间，都将集合类型视为集合。|

上表按优先级从高到低的顺序列出集合接口。 现举例说明这样列出的含义：如果一个类型同时实现 <xref:System.Collections.IList> 和泛型 <xref:System.Collections.Generic.IEnumerable%601>，则集合将按照 <xref:System.Collections.IList> 规则进行序列化和反序列化：

- 在反序列化时，将通过调用无参数的构造函数来反序列化所有集合，序列化程序在序列化和反序列化过程中将集合类型视为集合。

- 如果同一泛型集合接口实现多次（例如，如果一个类型既实现 <xref:System.Collections.Generic.ICollection%601> 的泛型 `Integer` ，也实现 <xref:System.Collections.Generic.ICollection%601> 的泛型 <xref:System.String>），并且找不到更高优先级的接口，则该集合将不会被视为有效集合。

- 集合类型可以应用 <xref:System.SerializableAttribute> 属性，并且可以实现 <xref:System.Runtime.Serialization.ISerializable> 接口。 两者都将被忽略。 但是，如果类型未充分满足集合类型要求（例如，缺少 `Add` 方法），则该类型将不会被视为集合类型，因此会使用 <xref:System.SerializableAttribute> 属性和 <xref:System.Runtime.Serialization.ISerializable> 接口来确定类型是否可以序列化。

- 为了自定义集合而向其应用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性会去掉上面的 <xref:System.SerializableAttribute> 回退机制。 在这种情况下，如果自定义集合不满足集合类型要求，将会引发 <xref:System.Runtime.Serialization.InvalidDataContractException> 异常。 异常字符串通常包含说明给定类型不被视为有效集合（无 `Add` 方法、没有无参数的构造函数等）的原因的信息，因此，为调试目的应用该属性通常很有用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 。

### <a name="collection-naming"></a>集合命名

以下是集合命名规则的列表：

- 所有字典集合数据协定以及包含基元类型的列表集合数据协定的默认命名空间均为， `http://schemas.microsoft.com/2003/10/Serialization/Arrays` 除非使用命名空间重写。 为此，映射到内置 XSD 类型的类型以及 `char`、 `Timespan`和 `Guid` 类型都将被视为基元。

- 包含非基元类型的集合类型的默认命名空间如果未使用 Namespace 重写，则与集合中包含的类型的数据协定命名空间相同。

- 列表集合数据协定的默认名称如果未使用 Name 重写，则是“ArrayOf”字符串与集合中包含类型的数据协定名称的组合。 例如，某个整数泛型列表的数据协定名称是“ArrayOfint”。 请记住， `Object` 的数据协定名称是“anyType”，因此诸如 <xref:System.Collections.ArrayList> 这样的非泛型列表的数据协定名称是“ArrayOfanyType”。

字典集合数据协定的默认名称如果未使用 `Name`重写，则是“ArrayOfKeyValueOf”字符串与键类型的数据协定名称以及值类型的数据协定名称（按此顺序）的组合。 例如，String 和 Integer 的泛型字典的数据协定名称是“ArrayOfKeyValueOfstringint”。 此外，如果键或值类型不是基元类型，则键和值类型的数据协定命名空间的命名空间哈希将会追加到名称的末尾。 有关命名空间哈希的详细信息，请参阅[数据协定名称](data-contract-names.md)。

每个字典集合数据协定都有一个表示字典中的一项的伴随数据协定。 除“ArrayOf”前缀外，它的名称与字典数据协定相同，并且命名空间也与字典数据协定相同。 例如，对于“ArrayOfKeyValueOfstringint”字典数据协定，“KeyValueofstringint”数据协定表示字典中的一项。 您可以使用 `ItemName` 属性来自定义该数据协定的名称，具体说明请见下一部分。

[Data Contract Names](data-contract-names.md)中介绍的泛型类型命名规则完全适用于集合类型，也就是说，你可以使用 Name 内的大括号来表示泛型类型参数。 但是，括号内的数字指泛型参数，而不是指集合中包含的类型。

## <a name="collection-customization"></a>集合自定义

<xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性的下列用法是被禁止的，这些用法会导致 <xref:System.Runtime.Serialization.InvalidDataContractException> 异常：

- 向已应用 <xref:System.Runtime.Serialization.DataContractAttribute> 属性的类型或者它的某个派生类型应用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性。

- 向实现 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 接口的类型应用 <xref:System.Xml.Serialization.IXmlSerializable> 属性。

- 向非集合类型应用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性。

- 试图对应用于非字典类型的 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> 属性设置 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> 或 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 。

### <a name="polymorphism-rules"></a>多态性规则

如前所述，使用 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性自定义集合可能会影响集合的可互换性。 如果两个自定义集合类型的名称、命名空间、项名称以及键和值名称（如果它们是字典集合）均匹配，则只能被视为等效。

由于自定义，可能意外地在本应使用某个集合数据协定的地方使用另一个集合数据协定。 应避免这种情况。 请参见以下类型。

[!code-csharp[c_collection_types_in_data_contracts#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#11)]
[!code-vb[c_collection_types_in_data_contracts#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#11)]

在本例中，可以将 `Marks1` 的实例指定给 `testMarks`。 但是，不应使用 `Marks2` ，因为其数据协定不被视为与 `IList<int>` 数据协定等效。 数据协定名称为 "Marks2" 而不是 "ArrayOfint"，重复元素名称为 " \<mark> "，而不是 " \<int> "。

下表中的规则适用于集合的多态指定。

|声明的类型|指定非自定义集合|指定自定义集合|
|-------------------|--------------------------------------------|---------------------------------------|
|对象|协定名称序列化。|协定名称序列化。<br /><br /> 使用自定义。|
|集合接口|协定名称不序列化。|协定名称不序列化。<br /><br /> 不使用自定义。\*|
|非自定义集合|协定名称不序列化。|协定名称序列化。<br /><br /> 使用自定义。**|
|自定义集合|协定名称序列化。 不使用自定义。\*\*|协定名称序列化。<br /><br /> 使用已分配类型的自定义。\*\*|

\*对于 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类，在这种情况下将使用自定义。 在这种情况下 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类还将序列化实际类型名称，以便反序列化按预期的方式工作。

\*\*这种情况会导致架构无效的实例，因此应避免这样做。

在序列化协定名称的情况下，指定的集合类型应当在已知类型列表中。 反之亦然：在不序列化协定名称的情况下，不需要将该类型添加到已知类型列表中。

可以将派生类型的数组指定给基类型数组。 在这种情况下，会针对每个重复元素序列化派生类型的协定名称。 例如，如果类型 `Book` 派生自类型 `LibraryItem`，则您可以将 `Book` 的数组指定给 `LibraryItem`的数组。 这不适用于其他集合类型。 例如，您不能将 `Generic List of Book` 指定给 `Generic List of LibraryItem`。 但可以指定包含 `Generic List of LibraryItem` 实例的 `Book` 。 在使用数组和非数组的情况下， `Book` 都应当在已知类型列表中。

## <a name="collections-and-object-reference-preservation"></a>集合和对象引用保存

当序列化程序在保存对象引用的模式下工作时，对象引用保存也适用于集合。 具体而言，会为整个集合以及集合中包含的各个项保存对象标识。 对于字典，会为键/值对对象以及各个键和值对象保存对象标识。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute>
