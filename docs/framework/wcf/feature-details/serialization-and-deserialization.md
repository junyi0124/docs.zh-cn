---
title: 序列化和反序列化
description: 了解 WCF 序列化引擎，该引擎可在两个方向上转换 .NET Framework 对象和 XML。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3d71814c-bda7-424b-85b7-15084ff9377a
ms.openlocfilehash: 94328fc2b2d008202221bfb4c05132e1286666e3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253942"
---
# <a name="serialization-and-deserialization"></a>序列化和反序列化

Windows Communication Foundation (WCF) 包括新的序列化引擎 <xref:System.Runtime.Serialization.DataContractSerializer> 。 <xref:System.Runtime.Serialization.DataContractSerializer>双向 .NET Framework 对象和 XML 之间进行转换。 本主题说明序列化程序的工作原理。  
  
 序列化 .NET Framework 对象时，序列化程序了解各种序列化编程模型，包括新的 *数据协定* 模型。 有关支持类型的完整列表，请参阅 [Types Supported by the Data Contract Serializer](types-supported-by-the-data-contract-serializer.md)。 有关数据协定的介绍，请参阅 [Using Data Contracts](using-data-contracts.md)。  
  
 当对 XML 进行反序列化时，序列化程序使用 <xref:System.Xml.XmlReader> 和 <xref:System.Xml.XmlWriter> 类。 它还支持 <xref:System.Xml.XmlDictionaryReader> 和 <xref:System.Xml.XmlDictionaryWriter> 类，以便在某些情况下（例如使用 WCF 二进制 XML 格式时），使其能够生成优化的 XML。  
  
 WCF 还包括一个伴随序列化程序 <xref:System.Runtime.Serialization.NetDataContractSerializer> 。 <xref:System.Runtime.Serialization.NetDataContractSerializer>：

* 为 ***而不** 是安全的。 有关详细信息，请参阅 [BinaryFormatter security guide （安全指南](../../../standard/serialization/binaryformatter-security-guide.md)）。
_ 与 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 和序列化程序类似， <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> 因为它还会发出 .NET Framework 类型名称作为序列化数据的一部分。
* 当在序列化和反序列化结束时共享相同的类型时，使用。

 <xref:System.Runtime.Serialization.DataContractSerializer>和都 <xref:System.Runtime.Serialization.NetDataContractSerializer> 派生自公共基类 <xref:System.Runtime.Serialization.XmlObjectSerializer> 。  
  
> [!WARNING]
> <xref:System.Runtime.Serialization.DataContractSerializer> 将包含带小于 20 的十六进制值的控制字符序列化为 XML 实体。 在将此类数据发送到 WCF 服务时，这可能会导致非 WCF 客户端出现问题。  
  
## <a name="creating-a-datacontractserializer-instance"></a>创建 DataContractSerializer 实例  

 构造 <xref:System.Runtime.Serialization.DataContractSerializer> 的实例是一个重要步骤。 完成构造后，将不能够更改任何设置。  
  
### <a name="specifying-the-root-type"></a>指定根类型  

  根类型是序列化或反序列化实例的类型。 <xref:System.Runtime.Serialization.DataContractSerializer> 有许多构造函数重载，但必须使用 `type` 参数提供至少一个根类型。  
  
 为某个根类型创建的序列化程序不能用于序列化（或反序列化）其他类型，除非该类型是从根类型派生的。 下面的示例演示了两个类。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#1)]
 [!code-vb[c_StandaloneDataContractSerializer#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#1)]  
  
 此代码构造 `DataContractSerializer` 的一个实例，它仅可用于序列化或反序列化 `Person` 类的实例。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#2)]
 [!code-vb[c_StandaloneDataContractSerializer#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#2)]  
  
### <a name="specifying-known-types"></a>指定已知类型  

 如果在进行序列化的类型中涉及多态性，并且尚未使用 <xref:System.Runtime.Serialization.KnownTypeAttribute> 特性或某种其他机制进行处理，则必须使用 `knownTypes` 参数将可能的已知类型的列表传递给序列化程序的构造函数。 有关已知类型的详细信息，请参阅 [数据协定已知类型](data-contract-known-types.md)。  
  
 下面的示例演示 `LibraryPatron`类，该类包含特定类型 `LibraryItem`的集合。 第二个类定义 `LibraryItem` 类型。 第三个和第四个类（`Book` 和 `Newspaper`）从 `LibraryItem` 类继承。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#3)]  
 [!code-vb[c_StandaloneDataContractSerializer#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#3)]  
  
 下面的代码构造一个使用 `knownTypes` 参数的序列化程序的实例。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#4)]
 [!code-vb[c_StandaloneDataContractSerializer#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#4)]  
  
### <a name="specifying-the-default-root-name-and-namespace"></a>指定默认根名称和命名空间  

 通常，在对对象进行序列化时，将根据数据协定名称和命名空间确定最外面的 XML 元素的默认名称和命名空间。 所有内部元素的名称将根据数据成员名称来确定，这些元素的命名空间是数据协定的命名空间。 下面的示例设置 `Name` 和 `Namespace` 类的构造函数中的 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 值。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#5)]
 [!code-vb[c_StandaloneDataContractSerializer#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#5)]  
  
 对 `Person` 类的实例进行序列化将生成类似如下的 XML。  
  
```xml  
<PersonContract xmlns="http://schemas.contoso.com">  
  <AddressMember>  
    <StreetMember>123 Main Street</StreetMember>  
   </AddressMember>  
</PersonContract>  
```  
  
 但是，可以通过将 `rootName` 和 `rootNamespace` 参数的值传递到 <xref:System.Runtime.Serialization.DataContractSerializer> 构造函数，自定义根元素的默认名称和命名空间。 注意， `rootNamespace` 不会影响对应于数据成员的所包含元素的命名空间， 而只是影响最外面元素的命名空间。  
  
 可以作为字符串或 <xref:System.Xml.XmlDictionaryString> 类的实例来传递这些值，从而允许使用二进制 XML 格式对其进行优化。  
  
### <a name="setting-the-maximum-objects-quota"></a>设置最大对象配额  

 一些 `DataContractSerializer` 构造函数重载具有 `maxItemsInObjectGraph` 参数。 此参数确定序列化程序在单个 <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> 方法调用中序列化或反序列化的对象的最大数目。 （该方法总是读取一个根对象，但此对象的数据成员中可以具有其他对象。 这些对象可能包含其他对象，依此类推。 ) 默认值为65536。 请注意，当序列化或反序列化数组时，每个数组项都计为一个单独的对象。 此外还应注意，一些对象可以有大内存表示形式，因此，单独使用此配额可能不足以防范拒绝服务攻击。 有关详细信息，请参阅 [数据的安全注意事项](security-considerations-for-data.md)。 如果需要增加此配额以至超出默认值，则一定要在发送（序列化）和接收（反序列化）方同时增加此配额，原因是在读取和写入数据时会此配额同时应用于发送方和接收方。  
  
### <a name="round-trips"></a>往返行程  

 在一次操作中对对象进行反序列化和重新序列化时将发生往返行程  。 因此，往返行程是从 XML 到对象实例，然后再返回到 XML 流。  
  
 一些 `DataContractSerializer` 构造函数重载具有 `ignoreExtensionDataObject` 参数，该参数默认设置为 `false` 。 在此默认模式中，对于一个往返行程，可以将数据从数据协定的较新版本发送到较旧版本然后再返回到较新版本而不会出现任何损失，前提是数据协定实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。 例如，假设 `Person` 数据协定的版本 1 包含 `Name` 和 `PhoneNumber` 数据成员，并且版本 2 添加 `Nickname` 成员。 如果在从版本 2 向版本 1 发送信息时实现了 `IExtensibleDataObject` ，则会存储 `Nickname` 数据，并在再次序列化数据时重新发出这些数据，因此，在往返行程中不会出现数据丢失。 有关详细信息，请参阅 [向前兼容的数据协定](forward-compatible-data-contracts.md) 和 [数据协定版本控制](data-contract-versioning.md)。  
  
#### <a name="security-and-schema-validity-concerns-with-round-trips"></a>往返行程的安全性和架构有效性问题  

 往返行程可能会涉及到一些安全性问题。 例如，反序列化和存储大量外来数据可能存在安全风险。 重新发出无法验证的数据可能会存在安全问题，尤其是在涉及数字签名的情况下。 例如，在前面的方案中，版本 1 终结点可能会对包含恶意数据的 `Nickname` 值进行签名。 最后，还可能存在架构有效性问题：终结点可能需要始终发出严格符合其声明的协定并且没有任何额外值的数据。 在前面的示例中，版本 1 终结点的协定声明该终结点仅发出 `Name` 和 `PhoneNumber`，并且如果正在使用构造验证，则发出额外的 `Nickname` 值将导致验证失败。  
  
#### <a name="enabling-and-disabling-round-trips"></a>启用和禁用往返行程  

 要关闭往返行程，请不要实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。 如果您无法控制相应的类型，则将 `ignoreExtensionDataObject` 参数设置为 `true` 也可获得同样的效果。  
  
### <a name="object-graph-preservation"></a>对象图保留  

 通常，序列化程序并不关心对象标识，如在下面的代码中所示。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#6)]
 [!code-vb[c_StandaloneDataContractSerializer#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#6)]  
  
 下面的代码创建一份订单。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#7)]
 [!code-vb[c_StandaloneDataContractSerializer#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#7)]  
  
 请注意，将 `billTo` 和 `shipTo` 字段设置为同一个对象实例。 但是，生成的 XML 会复制重复的信息，并与下面的 XML 类似。  
  
```xml  
<PurchaseOrder>  
  <billTo><street>123 Main St.</street></billTo>  
  <shipTo><street>123 Main St.</street></shipTo>  
</PurchaseOrder>  
```  
  
 不过，此方法具有以下可能不需要的特征：  
  
- 性能。 复制数据的效率低。  
  
- 循环引用。 如果对象引用自身，甚至通过其他对象引用自身，则通过复制进行序列化会导致无限循环。 （如果发生这种状况，序列化程序将引发 <xref:System.Runtime.Serialization.SerializationException> 。）  
  
- 语义。 有时，一定要记住这一点：两个引用指向的是同一个对象而不是两个相同的对象。  
  
 有关这些原因，一些 `DataContractSerializer` 构造函数重载具有 `preserveObjectReferences` 参数（默认值为 `false`）。 将此参数设置为时 `true` ，将使用一种特殊的编码对象引用方法，只使用 WCF 识别的对象。 当设置为 `true`时，XML 代码示例现在如下所示。  
  
```xml  
<PurchaseOrder ser:id="1">  
  <billTo ser:id="2"><street ser:id="3">123 Main St.</street></billTo>  
  <shipTo ser:ref="2"/>  
</PurchaseOrder>  
```  
  
 "Ser" 命名空间引用标准序列化命名空间 `http://schemas.microsoft.com/2003/10/Serialization/` 。 每一段数据只进行一次序列化并获得一个 ID 号，后续使用会导致引用已序列化的数据。  
  
> [!IMPORTANT]
> 如果“id”和“ref”属性同时存在于数据协定 `XMLElement`中，则接受“ref”属性，而忽略“id”属性。  
  
 了解此模式的限制是很重要的：  
  
- `DataContractSerializer` 在 `preserveObjectReferences` 设置为 `true` 的情况下生成的 XML 与任何其他技术都无法进行交互，仅可以由另一个其 `DataContractSerializer` 也设置为 `preserveObjectReferences` 的 `true`实例进行访问。  
  
- 元数据（架构）不支持此功能。 生成的架构仅对 `preserveObjectReferences` 设置为 `false`的情况有效。  
  
- 此功能可能导致序列化和反序列化进程运行速度减慢。 尽管不必复制数据，但是在此模式中必须执行额外的对象比较。  
  
> [!CAUTION]
> 当启用 `preserveObjectReferences` 模式时，需要将 `maxItemsInObjectGraph` 值设置为正确的配额，这一点特别重要。 由于在此模式中处理数组的方式方面的原因，攻击者很容易构造一条小的恶意消息来造成内存大量消耗（仅通过 `maxItemsInObjectGraph` 配额来限制）。  
  
### <a name="specifying-a-data-contract-surrogate"></a>指定数据协定代理项  

 一些 `DataContractSerializer` 构造函数重载具有 `dataContractSurrogate` 参数，该参数可以设置为 `null`。 此外，可以使用它来指定数据协定代理项 ，数据协定代理项是一种实现 <xref:System.Runtime.Serialization.IDataContractSurrogate> 接口的类型。 然后可以使用该接口来自定义序列化和反序列化进程。 有关详细信息，请参阅 [数据协定代理](../extending/data-contract-surrogates.md)项。  
  
## <a name="serialization"></a>序列化  

 下面的信息适用于从 <xref:System.Runtime.Serialization.XmlObjectSerializer>继承的任何类，包括 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类。  
  
### <a name="simple-serialization"></a>简单序列化  

 对对象进行序列化最基本的方法是将其传递到 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObject%2A> 方法。 该方法有三个重载，每个重载分别用于写入到 <xref:System.IO.Stream>、 <xref:System.Xml.XmlWriter>或 <xref:System.Xml.XmlDictionaryWriter>。 使用 <xref:System.IO.Stream> 重载时，输出是采用 UTF-8 编码的 XML。 使用 <xref:System.Xml.XmlDictionaryWriter> 重载时，序列化程序会针对二进制 XML 优化其输出。  
  
 使用方法时 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObject%2A> ，序列化程序使用包装元素的默认名称和命名空间，并将其与内容一起写入 (参阅前面的 "指定默认根名称和命名空间" 部分) 。  
  
 下面的示例演示如何使用 <xref:System.Xml.XmlDictionaryWriter>进行写入。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#8)]
 [!code-vb[c_StandaloneDataContractSerializer#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#8)]  
  
 这将生成类似于如下所示的 XML。  
  
```xml  
<Person>  
  <Name>Jay Hamlin</Name>  
  <Address>123 Main St.</Address>  
</Person>  
```  
  
### <a name="step-by-step-serialization"></a>分步引导的序列化  

 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A>、 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A>和 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> 方法可分别用于写入结束元素、写入对象内容以及关闭包装元素。  
  
> [!NOTE]
> 这些方法没有 <xref:System.IO.Stream> 重载。  
  
 此分步引导的序列化具有两个常见用途。 一种用途是在 `WriteStartObject` 和 `WriteObjectContent`之间插入内容（例如属性或注释），如以下示例所示。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#9)]
 [!code-vb[c_StandaloneDataContractSerializer#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#9)]  
  
 这将生成类似于如下所示的 XML。  
  
```xml  
<Person serializedBy="myCode">  
  <Name>Jay Hamlin</Name>  
  <Address>123 Main St.</Address>  
</Person>  
```  
  
 另一种常见用途是完全避免使用 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> 和 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> ，并写入自己的自定义包装元素（或者甚至连同跳过写入包装），如以下代码中所示。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#10)]
 [!code-vb[c_StandaloneDataContractSerializer#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#10)]  
  
 这将生成类似于如下所示的 XML。  
  
```xml  
<MyCustomWrapper>  
  <Name>Jay Hamlin</Name>  
  <Address>123 Main St.</Address>  
</MyCustomWrapper>  
```  
  
> [!NOTE]
> 使用分步引导的序列化可能会导致架构无效的 XML。  
  
## <a name="deserialization"></a>反序列化  

 下面的信息适用于从 <xref:System.Runtime.Serialization.XmlObjectSerializer>继承的任何类，包括 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Runtime.Serialization.NetDataContractSerializer> 类。  
  
 对对象进行反序列化的最基本的方式是调用 <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> 方法重载之一。 该方法有三个重载，每个重载分别用于读取 <xref:System.Xml.XmlDictionaryReader>、 `XmlReader`或 `Stream`。 请注意， `Stream` 重载将创建不受任何配额保护的文本 <xref:System.Xml.XmlDictionaryReader> ，此重载仅应用于读取受信任的数据。  
  
 还请注意，必须将 `ReadObject` 方法返回的对象强制转换为适当的类型。  
  
 下面的代码构造 <xref:System.Runtime.Serialization.DataContractSerializer> 和 <xref:System.Xml.XmlDictionaryReader>的实例，然后对 `Person` 实例进行反序列化。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#11)]
 [!code-vb[c_StandaloneDataContractSerializer#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#11)]  
  
 在调用 <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> 方法之前，将 XML 读取器置于包装元素上或包装元素前面的非内容节点上。 可以通过调用 <xref:System.Xml.XmlReader.Read%2A> 或其派生项的 <xref:System.Xml.XmlReader> 方法并测试 <xref:System.Xml.XmlReader.NodeType%2A>来完成此操作，如以下代码所示。  
  
 [!code-csharp[c_StandaloneDataContractSerializer#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_standalonedatacontractserializer/cs/source.cs#12)]
 [!code-vb[c_StandaloneDataContractSerializer#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_standalonedatacontractserializer/vb/source.vb#12)]  
  
 请注意，在将读取器传递给 `ReadObject`之前，可以读取此包装元素上的属性。  
  
 使用简单的重载之一时 `ReadObject` ，反序列化程序会在包装元素上查找默认名称和命名空间 (参阅前面的 "指定默认根名称和命名空间" 一节 ) 并在找到未知元素时引发异常。 在上面的示例中，应有 `<Person>` 包装元素。 可调用 <xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> 方法来验证是否已将读取器定位在按预期命名的元素上。  
  
 有一种方法可以用来禁用此包装元素名称检查；一些 `ReadObject` 方法的重载采用布尔参数 `verifyObjectName`，该参数默认设置为 `true` 。 当该参数设置为 `false`时，包装元素的名称和命名空间将被忽略。 这对于读取使用先前描述的分步引导的序列化机制写入的 XML 是有用的。  
  
## <a name="using-the-netdatacontractserializer"></a>使用 NetDataContractSerializer  

 与之间的主要差异在于 `DataContractSerializer` <xref:System.Runtime.Serialization.NetDataContractSerializer> `DataContractSerializer` 使用数据协定名称，而在 `NetDataContractSerializer` 序列化的 XML 中输出完整 .NET Framework 程序集和类型名称。 这意味着必须在序列化终结点和反序列化终结点之间共享完全相同的类型。 这也同时意味着不需要对 `NetDataContractSerializer` 使用已知类型机制，因为要反序列化的确切类型始终是已知的。  
  
 但是，会出现以下几个问题：  
  
- 安全性。 在要反序列化的 XML 中找到的任何类型都会加载。 有人可以利用这一点来强制加载恶意类型。 仅在使用 `NetDataContractSerializer` 序列化联编程序 *时（使用* 属性或构造函数参数）才应将 <xref:System.Runtime.Serialization.NetDataContractSerializer.Binder%2A> 用于不受信任的数据。 联编程序仅允许加载安全类型。 联编程序机制与 <xref:System.Runtime.Serialization> 命名空间中的类型使用的机制相同。  
  
- 版本管理。 在 XML 中使用完整的类型和程序集名称会严格限制对类型进行版本管理的方式。 以下内容不可更改：类型名称、命名空间、程序集名称和程序集版本。 通过将 <xref:System.Runtime.Serialization.NetDataContractSerializer.AssemblyFormat%2A> 属性或构造函数参数设置为 <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle.Simple> （而非 <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle.Full> 的默认值），可以允许程序集版本更改，但不允许泛型参数类型更改。  
  
- 互操作性。 由于 .NET Framework 类型和程序集名称包含在 XML 中，因此 .NET Framework 以外的平台不能访问生成的数据。  
  
- 性能。 写出类型和程序集名称会显著增加生成的 XML 的大小。  
  
 此机制类似于 .NET Framework 远程处理 (所使用的二进制或 SOAP 序列化， <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 以及和 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>) 。  
  
 使用 `NetDataContractSerializer` 与使用 `DataContractSerializer`类似，但存在以下区别：  
  
- 构造函数不要求指定根类型。 可以使用 `NetDataContractSerializer`的相同实例对任何类型进行序列化。  
  
- 构造函数不接受已知类型的列表。 如果将类型名称序列化为 XML，则不需要已知类型机制。  
  
- 构造函数不接受数据协定代理项， 而是接受一个名为 <xref:System.Runtime.Serialization.ISurrogateSelector> 的 `surrogateSelector` 参数（映射到 <xref:System.Runtime.Serialization.NetDataContractSerializer.SurrogateSelector%2A> 属性）。 这是旧式代理项机制。  
  
- 构造函数接受 `assemblyFormat` 的一个名为 <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle> 的参数，该参数映射到 <xref:System.Runtime.Serialization.NetDataContractSerializer.AssemblyFormat%2A> 属性。 如前所述，这可以用于增强序列化程序的版本管理功能。 这与二进制或 SOAP 序列化中的 <xref:System.Runtime.Serialization.Formatters.FormatterAssemblyStyle> 机制相同。  
  
- 构造函数接受一个名为 <xref:System.Runtime.Serialization.StreamingContext> 的 `context` 参数，该参数映射到 <xref:System.Runtime.Serialization.NetDataContractSerializer.Context%2A> 属性。 可以使用该参数将信息传递到要序列化的类型中。 此用法与其他 <xref:System.Runtime.Serialization.StreamingContext> 类中使用的 <xref:System.Runtime.Serialization> 机制的用法相同。  
  
- <xref:System.Runtime.Serialization.NetDataContractSerializer.Serialize%2A> 和 <xref:System.Runtime.Serialization.NetDataContractSerializer.Deserialize%2A> 方法是 <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObject%2A> 和 <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> 方法的别名。 这些别名可以为二进制或 SOAP 序列化提供更为一致的编程模型。  
  
 有关这些功能的详细信息，请参阅 [二进制序列化](../../../standard/serialization/binary-serialization.md)。  
  
 `NetDataContractSerializer` 和 `DataContractSerializer` 使用的 XML 格式通常是不兼容的。 也就是说，不支持尝试使用这些序列化程序的一种进行序列化而使用另一种序列化程序进行反序列化的情况。  
  
 另请注意，不 `NetDataContractSerializer` 会输出对象图中每个节点的完整 .NET Framework 类型和程序集名称。 仅在有歧义的地方才会输出上述信息。 也就是说，它是在根对象级别进行输出并且是针对任何多态情况。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.NetDataContractSerializer>
- <xref:System.Runtime.Serialization.XmlObjectSerializer>
- [二进制序列化](../../../standard/serialization/binary-serialization.md)
- [数据协定序列化程序支持的类型](types-supported-by-the-data-contract-serializer.md)
