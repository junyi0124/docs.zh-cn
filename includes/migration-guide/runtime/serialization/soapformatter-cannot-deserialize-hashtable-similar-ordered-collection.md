---
ms.openlocfilehash: 71cc7119710a2b381626dd9cf4ab66265eb3deba
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496520"
---
### <a name="soapformatter-cannot-deserialize-hashtable-and-similar-ordered-collection-objects"></a>SoapFormatter 无法反序列化哈希表和类似有序的集合对象

#### <a name="details"></a>详细信息

<xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter?displayProperty=fullName> 不保证在一个 .NET Framework 版本下进行序列化的对象能够在其他版本下成功反序列化。 具体而言，某些有序集合（例如 <xref:System.Collections.Hashtable?displayProperty=fullName>）已在 4.0 和 4.5 中添加了成员，如果这些类型的对象在 .NET Framework 4.5 中进行了序列化，它们在 .NET Framework 4.0 中将无法反序列化。 请注意，如果序列化的数据在同一 .NET Framework 版本中进行序列化和反序列化，将不会发生任何问题。

#### <a name="suggestion"></a>建议

应使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> 序列化或 <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName> 替换 <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter?displayProperty=fullName> 序列化以适应 .NET Framework 的更改。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object,System.Runtime.Remoting.Messaging.Header[])?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream,System.Runtime.Remoting.Messaging.HeaderHandler)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object)`
- `M:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Serialize(System.IO.Stream,System.Object,System.Runtime.Remoting.Messaging.Header[])`
- `M:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream)`
- `M:System.Runtime.Serialization.Formatters.Soap.SoapFormatter.Deserialize(System.IO.Stream,System.Runtime.Remoting.Messaging.HeaderHandler)`

-->
