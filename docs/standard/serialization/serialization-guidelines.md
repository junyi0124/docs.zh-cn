---
title: 序列化准则
description: 本文提供了在设计要序列化的 API 时应考虑的准则，并简要介绍了 .NET 提供的三个主要序列化技术。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serialization, guidelines
- binary serialization, guidelines
ms.assetid: ebbeddff-179d-443f-bf08-9c373199a73a
ms.openlocfilehash: 110efce0bd7fae1a4f39f5d879496bf541ffe667
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722149"
---
# <a name="serialization-guidelines"></a>序列化准则

本文列出了在设计要序列化的 API 时要考虑的准则。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

 .NET 提供了针对各种序列化方案进行优化的三种主要序列化技术。 下表列出了这些技术以及与这些技术相关的主要 .NET 类型。

|技术|相关的类|说明|
|----------------|----------------------|-----------|
|数据协定序列化|<xref:System.Runtime.Serialization.DataContractAttribute><br /><br /> <xref:System.Runtime.Serialization.DataMemberAttribute><br /><br /> <xref:System.Runtime.Serialization.DataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.NetDataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer><br /><br /> <xref:System.Runtime.Serialization.ISerializable>|常规持久性<br /><br /> Web 服务<br /><br /> JSON|
|XML 序列化|<xref:System.Xml.Serialization.XmlSerializer>|具有完全控制的 <br />XML 格式|
|运行时 -序列化（二进制和 SOAP）|<xref:System.SerializableAttribute><br /><br /> <xref:System.Runtime.Serialization.ISerializable><br /><br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter><br /><br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|.NET 远程处理|

 设计新的类型时，应决定这些类型需要支持哪种技术（如果有的话）。 以下准则介绍了如何做出此决定以及如何提供这类支持。 这些准则并不表示帮助您选择在实现应用程序或库时应使用哪种序列化技术。 这些准则与 API 设计不是直接相关的，因此它们不在本主题的讨论范围内。

## <a name="guidelines"></a>准则

- 设计新类型时请务必考虑序列化。

  对于任何类型来说，序列化都是一个重要的设计考虑事项，因为程序可能需要持久保持或传输类型的实例。

### <a name="choosing-the-right-serialization-technology-to-support"></a>选择要支持的合适的序列化技术

 任何给定类型均可支持一种或多种序列化技术，或者不支持任何序列化技术。

- 如果可能需要在 Web 服务中持久保持或使用类型的实例，则应考虑支持数据协定序列化。

- 如果需要对序列化类型时生成的 XML 格式具有更多控制，则应考虑改为支持 XML 序列化，或者在支持数据协定序列化之外还支持 XML 序列化。

     这对于某些需要使用数据协定序列化所不支持的 XML 构造（例如，用于生成 XML 特性）的互操作性方案可能是必需的。

- 如果类型实例需要越过 .NET 远程处理边界传递，则应考虑支持运行时序列化。

- 应避免仅仅因为常规持久性原因而支持运行时序列化或 XML 序列化， 而应首选数据协定序列化。

#### <a name="data-contract-serialization"></a>数据协定序列化

 通过将 <xref:System.Runtime.Serialization.DataContractAttribute> 应用到类型，并将 <xref:System.Runtime.Serialization.DataMemberAttribute> 应用到类型的成员（字段和属性），类型可支持数据协定序列化。

 [!code-csharp[SerializationGuidelines#1](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#1)]
 [!code-vb[SerializationGuidelines#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#1)]

1. 如果可以在部分信任模式下使用类型，则考虑将类型的数据成员标记为公共的。 在完全信任模式下，数据协定序列化程序可对非公共类型和成员进行序列化和反序列化，但在部分信任模式下，仅可对公共成员进行序列化和反序列化。

2. 请务必在包含 Data-MemberAttribute 的所有属性上实现 getter 和 setter。 对于考虑可进行序列化的类型，数据协定序列化程序要求同时具备 getter 和 setter。 如果不会在部分信任模式下使用类型，则这两个属性访问器中的一个或两个都可以是非公共的。

     [!code-csharp[SerializationGuidelines#2](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#2)]
     [!code-vb[SerializationGuidelines#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#2)]

3. 对于反序列化实例的初始化，应考虑使用序列化回调。

     反序列化对象时，不调用任何构造函数。 因此，在正常构造期间执行的任何逻辑都需要作为一个序列化回调实现。

     [!code-csharp[SerializationGuidelines#3](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#3)]
     [!code-vb[SerializationGuidelines#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#3)]

     <xref:System.Runtime.Serialization.OnDeserializedAttribute> 属性是最常用的回调属性。 此系列中的其他属性还包括 <xref:System.Runtime.Serialization.OnDeserializingAttribute>、<xref:System.Runtime.Serialization.OnSerializingAttribute> 和 <xref:System.Runtime.Serialization.OnSerializedAttribute>。 这些属性可分别用来标记在反序列化之前、序列化之前以及序列化之后执行的回调。

4. 请考虑使用 <xref:System.Runtime.Serialization.KnownTypeAttribute> 指示在反序列化复杂对象关系图时应使用的具体类型。

     例如，如果使用一个抽象类表示反序列化数据成员的类型，则序列化程序将需要已知类型信息，以便决定要实例化并分配给成员的具体类型。 如果未使用相关属性指定已知类型，则需要将已知类型显式传递给序列化程序（可通过将已知类型传递给序列化程序构造函数来执行此操作），或者需要在配置文件中指定已知类型。

     [!code-csharp[SerializationGuidelines#4](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#4)]
     [!code-vb[SerializationGuidelines#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#4)]

     在无法通过静态方式了解已知类型列表的情况下（当编译 Person 类时），KnownTypeAttribute 还可指向一个在运行时返回已知类型列表的方法 。

5. 创建或更改可序列化的类型时，请务必考虑向后兼容性和向前兼容性。

     请记住，类型的未来版本的序列化流可反序列化到类型的当前版本，反之亦然。 您一定要清楚，数据成员（甚至对于私有成员和内部成员）不能更改其名称和类型，甚至不能更改其在类型的未来版本中的顺序，除非特别留意将使用显式参数的协定保存到数据协定属性。在对可序列化的类型进行更改时，测试序列化的兼容性。 尝试将新版本反序列化为旧版本，以及将旧版本反序列化为新版本。

6. 考虑实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口以允许在类型的两个不同版本之间进行往返。

     序列化程序可通过此接口确保在往返期间不丢失任何数据。 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> 属性可存储类型的未来版本中不为当前版本所知的任何数据。 在随后序列化当前版本并将其反序列化为未来版本时，可通过 ExtensionData 属性值在序列化流中使用附加数据。

     [!code-csharp[SerializationGuidelines#5](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#5)]
     [!code-vb[SerializationGuidelines#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#5)]

     有关详细信息，请参阅[向前兼容的数据协定](../../framework/wcf/feature-details/forward-compatible-data-contracts.md)。

#### <a name="xml-serialization"></a>XML 序列化

 数据协定序列化是 .NET Framework 中的主要（默认）序列化技术，但也存在数据协定序列化不支持的序列化情况。 例如，数据协定序列化无法让您完全控制序列化程序生成或使用的 XML 的形状。 如果要求此类精细控制，则必须使用 XML 序列化，而且需要设计类型以支持此序列化技术。

1. 应避免专门针对 XML 序列化设计类型，除非您有很充分的理由要控制所生成的 XML 的形状。 此序列化技术已由上一节讨论的数据协定序列化所取代。

     换句话说，不要将 <xref:System.Xml.Serialization> 命名空间中的属性应用到新类型，除非你清楚该类型将会用于 XML 序列化。 以下示例展示如何使用 System.Xml.Serialization 来控制生成的 XML 的形状。

     [!code-csharp[SerializationGuidelines#6](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#6)]
     [!code-vb[SerializationGuidelines#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#6)]

2. 如果通过应用 XML 序列化属性所提供的对于序列化 XML 的形状的控制还无法满足你的需要，则可考虑实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口。 利用此接口的两种方法（<xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> 和 <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>），你可以完全控制序列化的 XML 流。 还可以通过应用 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性来控制为类型生成的 XML 架构。

#### <a name="runtime-serialization"></a>运行时序列化

 运行时序列化是 .NET 远程处理所使用的一项技术。 如果你认为将会使用 .NET 远程处理传输类型，则确保类型支持运行时序列化。

 可通过应用 <xref:System.SerializableAttribute> 属性提供对运行时序列化的基本支持，更高级的方案涉及实现一个简单的运行时可序列化模式（实现 -<xref:System.Runtime.Serialization.ISerializable> 并提供一个序列化构造函数） 。

1. 如果您的类型将要用于 .NET 远程处理，则应考虑支持运行时序列化。 例如，<xref:System.AddIn> 命名空间使用 .NET 远程处理，因此在 System.AddIn 加载项之间交换的所有类型都需要支持运行时序列化。

     [!code-csharp[SerializationGuidelines#7](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#7)]
     [!code-vb[SerializationGuidelines#7](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#7)]

2. 如果需要完全控制序列化过程，则应考虑实现运行时可序列化模式。 例如，如果您需要在对数据进行序列化或反序列化时转换数据。

     此模式非常简单。 您需要执行的全部操作就是实现 <xref:System.Runtime.Serialization.ISerializable> 接口，并提供在反序列化对象时使用的特殊构造函数。

     [!code-csharp[SerializationGuidelines#8](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#8)]
     [!code-vb[SerializationGuidelines#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#8)]

3. 请务必保护序列化构造函数，并提供完全按照此处示例中的显示类型化和命名的两个参数。

     [!code-csharp[SerializationGuidelines#9](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#9)]
     [!code-vb[SerializationGuidelines#9](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#9)]

4. 请务必显式实现 ISerializable 成员。

     [!code-csharp[SerializationGuidelines#10](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#10)]
     [!code-vb[SerializationGuidelines#10](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#10)]

5. 请务必向 **ISerializable.GetObjectData** 实现应用链接要求。 这将确保只有完全受信任的核心和运行时序列化程序才有权访问成员。

     [!code-csharp[SerializationGuidelines#11](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#11)]
     [!code-vb[SerializationGuidelines#11](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#11)]

## <a name="see-also"></a>请参阅

- [使用数据协定](../../framework/wcf/feature-details/using-data-contracts.md)
- [数据协定序列化程序](../../framework/wcf/feature-details/data-contract-serializer.md)
- [数据协定序列化程序支持的类型](../../framework/wcf/feature-details/types-supported-by-the-data-contract-serializer.md)
- [二进制序列化](binary-serialization.md)
- [.NET 远程处理](/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [安全性和序列化](../../framework/misc/security-and-serialization.md)
