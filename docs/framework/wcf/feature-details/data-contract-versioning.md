---
title: 数据协定版本管理
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- versioning [WCF], data contracts
- versioning [WCF]
- data contracts [WCF], versioning
ms.assetid: 4a0700cb-5f5f-4137-8705-3a3ecf06461f
ms.openlocfilehash: 6f8623c9d8e9e7ba1f7c762c929f986b523c2f90
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96285195"
---
# <a name="data-contract-versioning"></a>数据协定版本管理

随着应用程序的发展，您也可能不得不更改服务使用的数据协定。 本主题说明如何管理数据协定的版本。 本主题介绍数据协定版本管理机制。 有关完整的概述和说明性的版本控制指南，请参阅 [最佳做法：数据协定版本控制](../best-practices-data-contract-versioning.md)。  
  
## <a name="breaking-vs-nonbreaking-changes"></a>重大更改与非重大更改  

 对数据协定的更改可能是重大更改，也可能是非重大更改。 对数据协定进行非重大更改时，使用较早版本协定的应用程序和使用较新版本协定的应用程序可以互相通信。 另一方面，如果进行重大更改，则会阻止单向或双向通信。  
  
 对类型的任何更改，只要不影响其传输方式和接收方式，都是非重大更改。 这类更改只更改基础类型，而不更改数据协定。 例如，如果更改一个字段的名称后，将 <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A> 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性设置为较早版本的名称，则属于非重大更改方式。 下面的代码演示数据协定的版本 1。  
  
 [!code-csharp[C_DataContractVersioning#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#1)]
 [!code-vb[C_DataContractVersioning#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#1)]  
  
 下面的代码演示非重大更改。  
  
 [!code-csharp[C_DataContractVersioning#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#2)]
 [!code-vb[C_DataContractVersioning#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#2)]  
  
 有些更改确实修改了传输的数据，但这些更改可能是重大更改，也可能不是重大更改。 下面的更改始终是重大更改：  
  
- 更改数据协定的 <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> 或 <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> 值。  
  
- 通过 <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性来更改数据成员的顺序。  
  
- 重命名数据成员。  
  
- 更改数据成员的数据协定。 例如，将数据成员的类型从整数更改为字符串，或者从数据协定名称为“Customer”的类型更改为数据协定名称为“Person”的类型。  
  
 下面的更改也可能是重大更改。  
  
## <a name="adding-and-removing-data-members"></a>添加或移除数据成员  

 大多数情况下，添加或移除数据成员不是重大更改，除非要求严格的架构验证（新实例针对旧架构进行验证）。  
  
 将具有额外字段的类型反序列化为具有缺失字段的类型时，将忽略额外的信息。  (它还可能出于往返目的而存储;有关详细信息，请参阅 [向前兼容的数据协定](forward-compatible-data-contracts.md)) 。  
  
 具有缺失字段的类型反序列化为具有额外字段的类型时，额外字段将保留其默认值，通常为零或 `null`。  (默认值可以更改，则为;有关详细信息，请参阅 [版本容错序列化回调](version-tolerant-serialization-callbacks.md)。 )   
  
 例如，您可以在客户端上使用 `CarV1` 类，在服务上使用 `CarV2` 类；也可以在服务上使用 `CarV1` 类，在客户端上使用 `CarV2` 类。  
  
 [!code-csharp[C_DataContractVersioning#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractversioning/cs/source.cs#3)]
 [!code-vb[C_DataContractVersioning#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractversioning/vb/source.vb#3)]  
  
 版本 2 终结点可以成功地向版本 1 终结点发送数据。 对版本 2 的 `Car` 数据协定进行序列化将生成 XML，如下所示。  
  
```xml  
<Car>  
    <Model>Porsche</Model>  
    <HorsePower>300</HorsePower>  
</Car>  
```  
  
 V1 上的反序列化引擎没有找到 `HorsePower` 字段的匹配数据成员，因而丢弃该数据。  
  
 同样，版本 1 终结点也可以向版本 2 终结点发送数据。 对版本 1 的 `Car` 数据协定进行序列化将生成 XML，如下所示。  
  
```xml  
<Car>  
    <Model>Porsche</Model>  
</Car>  
```  
  
 版本 2 反序列化程序不知道将 `HorsePower` 字段设置为何值，因为传入的 XML 中没有匹配数据。 该字段设置为默认值 0。  
  
## <a name="required-data-members"></a>必需的数据成员  

 通过将 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 的 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性设置为 `true`，可以将数据成员标记为必需的数据成员。 如果反序列化时缺少必需的数据，则会引发异常，而不是将数据成员设置为其默认值。  
  
 添加必需的数据成员是重大更改。 也就是说，新类型仍然可以发送到具有旧类型的终结点，但无法反向发送。 移除在任何早期版本中标记为必需成员的数据成员也是重大更改。  
  
 将 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性值从 `true` 更改为 `false` 不是重大更改；如果类型的任何早期版本都没有相应数据成员，将该属性值从 `false` 更改为 `true` 就可能是重大更改。  
  
> [!NOTE]
> 尽管 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 属性设置为 `true`，但传入数据可能为 null 或零，类型必须准备好处理这种可能的情况。 不要将 <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> 用作安全机制来防止不良传入数据。  
  
## <a name="omitted-default-values"></a>省略的默认值  

 尽管不) 建议将 DataMemberAttribute 属性的属性设置为，但也可以 (`EmitDefaultValue` `false` ，如 [数据成员默认值](data-member-default-values.md)中所述。 如果该属性设置为 `false`，而数据成员设置为其默认值（通常为 null 或零），则不会发出该数据成员。 这样，就在两个方面与不同版本中的必需数据成员不兼容：  
  
- 一个版本中具有必需数据成员的数据协定无法从该数据成员的 `EmitDefaultValue` 已设置为 `false` 的另一个版本接收默认值（null 或零）数据。  
  
- 已将 `EmitDefaultValue` 设置为 `false` 的必需数据成员不可用来序列化其默认值（null 或零），但它可在反序列化时接收其默认值。 这就形成了一个往返问题（数据可以读入，但随后无法写出同样的数据）。 因此，如果在一个版本中，`IsRequired` 为 `true` 而 `EmitDefaultValue` 为 `false`，则同样的组合应当应用到所有其他版本，任何数据协定版本都无法生成一个不会导致往返过程的值。  
  
## <a name="schema-considerations"></a>架构注意事项  

 有关为数据协定类型生成的架构的说明，请参阅 [数据协定架构引用](data-contract-schema-reference.md)。  
  
 WCF 为数据协定类型生成的架构不会对版本控制进行任何设置。 也就是说，从类型的某个版本中导出的架构仅包含该版本的数据成员。 实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口不会改变类型的架构。  
  
 默认情况下，数据成员是作为可选元素导出到架构的。 即，`minOccurs`（XML 属性）值设置为 0。 如果 `minOccurs` 设置为 1，则导出必需的数据成员。  
  
 如果要求严格遵从架构，许多视为非重大更改的更改实际上是重大更改。 在上面的示例中，仅具有 `CarV1` 元素的 `Model` 实例将针对 `CarV2` 架构（具有 `Model` 和 `Horsepower`，但它们都是可选的）进行验证。 但是，反过来并不成立：`CarV2` 实例无法针对 `CarV1` 架构进行验证。  
  
 关于往返过程，还有一些其他注意事项。 有关详细信息，请参阅 [向前兼容的数据协定](forward-compatible-data-contracts.md)中的 "架构注意事项" 部分。  
  
### <a name="other-permitted-changes"></a>其他允许的更改  

 实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口是非重大更改。 但是，在实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 的版本之前，类型版本不提供往返过程支持。 有关详细信息，请参阅[向前兼容的数据协定](forward-compatible-data-contracts.md)。  
  
## <a name="enumerations"></a>枚举  

 添加或移除枚举成员是重大更改。 更改枚举成员的名称是重大更改，除非使用 `EnumMemberAttribute` 属性将其协定名称保持为与旧版本中的名称相同。 有关详细信息，请参阅 [数据协定中的枚举类型](enumeration-types-in-data-contracts.md)。  
  
## <a name="collections"></a>集合  

 大多数集合更改是非重大更改，这是因为在数据协定模型中，大多数集合类型可以彼此互换。 但是，将非自定义集合更改为自定义集合是重大更改，反之亦然。 此外，更改集合的自定义设置（即，更改其数据协定名称和命名空间、重复元素名称、键元素名称以及值元素名称）也是重大更改。 有关集合自定义的详细信息，请参阅 [数据协定中的集合类型](collection-types-in-data-contracts.md)。  
更改集合内容的数据协定（例如，从整数列表更改为字符串列表）自然也是重大更改。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A>
- <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A>
- <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A>
- <xref:System.Runtime.Serialization.SerializationException>
- <xref:System.Runtime.Serialization.IExtensibleDataObject>
- [版本容错序列化回调](version-tolerant-serialization-callbacks.md)
- [最佳做法：数据协定版本管理](../best-practices-data-contract-versioning.md)
- [使用数据协定](using-data-contracts.md)
- [数据协定等效性](data-contract-equivalence.md)
- [向前兼容的数据协定](forward-compatible-data-contracts.md)
