---
ms.openlocfilehash: ccba3cf98a1ca9e199d9a48f00e254bf34b93f72
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497612"
---
### <a name="binaryformatter-can-fail-to-find-type-from-loadfrom-context"></a>BinaryFormatter 可能无法从 LoadFrom 上下文找到类型

#### <a name="details"></a>详细信息

自 .NET Framework 4.5 起，大量 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=fullName> 更改可能在使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> 反序列化 LoadFrom 上下文中负载的类型时导致反序列化出现差异。 这些更改由新的方式引起，<xref:System.Xml.Serialization.XmlSerializer?displayProperty=fullName> 现在加载了一种类型，可以在 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> 稍后尝试反序列化为该类型时，导致其他行为。 默认序列化活页夹虽然在某些情况下可以基于 XmlSerializer 旧行为工作，但它不会自动搜索 LoadFrom 上下文。 由于这些更改，当从其他上下文加载的程序集加载类型时，可能引发 <xref:System.IO.FileNotFoundException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

如果显示此异常，则 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=fullName> 的 <code>Binder</code> 属性可以设为将查找正确类型的自定义活页夹。<pre><code class="lang-csharp">var formatter = new BinaryFormatter { Binder = new TypeFinderBinder() }&#13;&#10;</code></pre>然后自定义活页夹：<pre><code class="lang-csharp">public class TypeFinderBinder : SerializationBinder&#13;&#10;{&#13;&#10;private static readonly string s_assemblyName = Assembly.GetExecutingAssembly().FullName;&#13;&#10;&#13;&#10;public override Type BindToType(string assemblyName, string typeName)&#13;&#10;{&#13;&#10;return Type.GetType(String.Format(CultureInfo.InvariantCulture, &quot;{0}, {1}&quot;, typeName, s_assemblyName));&#13;&#10;}&#13;&#10;}&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize(System.IO.Stream,System.Runtime.Remoting.Messaging.HeaderHandler)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter`
- `M:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize(System.IO.Stream)`
- `M:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize(System.IO.Stream,System.Runtime.Remoting.Messaging.HeaderHandler)`

-->
