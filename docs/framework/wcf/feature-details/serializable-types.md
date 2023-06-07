---
title: 可序列化类型
ms.date: 03/30/2017
ms.assetid: f1c8539a-6a79-4413-b294-896f0957b2cd
ms.openlocfilehash: 4ba5fb80b3a7f4149eb49aa838826f2792147dd1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253955"
---
# <a name="serializable-types"></a>可序列化类型

默认情况下，<xref:System.Runtime.Serialization.DataContractSerializer> 序列化所有公共可见类型。 类型的所有公共读/写属性和字段均被序列化。  
  
 可以通过对类型和成员应用 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性来更改默认行为。当您拥有不受您控制因而无法对其进行修改以添加属性的类型时，此功能可能会十分有用。 <xref:System.Runtime.Serialization.DataContractSerializer> 可识别这类“未标记”的类型。  
  
## <a name="serialization-defaults"></a>序列化默认设置  

 可以应用 <xref:System.Runtime.Serialization.DataContractAttribute> 和 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性以显式控制或自定义类型和成员的序列化。 此外，还可以将这些属性应用于私有字段。 但是，即使未用这些属性进行标记的类型也会进行序列化和反序列化。 适用的规则和例外如下：  
  
- <xref:System.Runtime.Serialization.DataContractSerializer> 使用新创建的类型的默认属性从不带属性的类型推断数据协定。  
  
- 除非对成员应用 `get` 属性 (Attribute)，否则所有公共字段以及具有公共 `set` 和 <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> 方法的属性 (Property) 都会序列化。  
  
- 序列化语义与 <xref:System.Xml.Serialization.XmlSerializer> 的语义类似。  
  
- 在未标记的类型中，仅序列化具有不带参数的构造函数的公共类型。 此规则的例外是用于 <xref:System.Runtime.Serialization.ExtensionDataObject> 接口的 <xref:System.Runtime.Serialization.IExtensibleDataObject>。  
  
- 只读字段、没有 `get` 或 `set` 方法的属性以及具有内部或私有 `set` 或 `get` 方法的属性不会进行序列化。 此类属性会被忽略，但不会引发异常（get-only 集合的情况除外）。  
  
- 会忽略 <xref:System.Xml.Serialization.XmlSerializer> 属性（如 `XmlElement`、`XmlAttribute`、`XmlIgnore`、`XmlInclude` 等）。  
  
- 如果未将 <xref:System.Runtime.Serialization.DataContractAttribute> 属性应用于某个给定类型，则序列化程序会忽略该类型中应用了 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性的所有成员。  
  
- 未使用 <xref:System.Runtime.Serialization.DataContractSerializer.KnownTypes%2A> 属性 (Attribute) 进行标记的类型中支持 <xref:System.Runtime.Serialization.DataContractAttribute> 属性 (Property)。 这包括对未标记类型上的 <xref:System.Runtime.Serialization.KnownTypeAttribute> 属性 (Attribute) 的支持。  
  
- 若要使公共成员、属性 (Property) 或字段“退出”序列化过程，请向该成员应用 <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> 属性 (Attribute)。  
  
## <a name="inheritance"></a>继承  

 未标记类型（没有 <xref:System.Runtime.Serialization.DataContractAttribute> 属性的类型）可以从具有此属性的类型继承；但是反过来则不允许：具有该属性的类型不能从未标记类型继承。 此规则主要用于确保向后兼容在 .NET Framework 早期版本中编写的代码。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Xml.Serialization.XmlSerializer>
- [数据协定序列化程序支持的类型](types-supported-by-the-data-contract-serializer.md)
