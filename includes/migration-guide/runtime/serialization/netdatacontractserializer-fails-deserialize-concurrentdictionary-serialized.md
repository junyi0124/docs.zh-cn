---
ms.openlocfilehash: 6c120f155660863ce5ae3cf5bd81ea858a68ef8d
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497277"
---
### <a name="netdatacontractserializer-fails-to-deserialize-a-concurrentdictionary-serialized-with-a-different-net-version"></a>NetDataContractSerializer 无法反序列化使用其他 .NET 版本序列化的 ConcurrentDictionary

#### <a name="details"></a>详细信息

根据设计，只有在序列化和反序列化端共享相同的 CLR 类型时，才能使用 <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName>。 因此，无法保证使用某个版本的 .NET Framework 序列化的对象可以由其他版本反序列化。<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName> 是一种类型，已知该类型如果使用 .NET Framework 4.5 或更早版本序列化并使用 .NET Framework 4.5.1 或更早版本反序列化，则无法正确反序列化。

#### <a name="suggestion"></a>建议

针对这个问题有很多可行的解决方法：<ul><li>升级序列化计算机以使用 .NET Framework 4.5.1。</li><li>使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=fullName> 而不是 <xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=fullName>，因为这并不指望在序列化端和反序列化端具有完全相同的 CLR 类型。</li><li>使用 <xref:System.Collections.Generic.Dictionary%602?displayProperty=fullName> 而不是 <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=fullName>，因为它不会显示出特定的 4.5-&gt;4.5.1 中断。</li></ul>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.Serialization.NetDataContractSerializer.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Runtime.Serialization.NetDataContractSerializer.Deserialize(System.IO.Stream)`

-->
