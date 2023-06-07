---
ms.openlocfilehash: afdf1e20db7dc564ddfb6028238604f97e00971a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614374"
---
### <a name="serialization-of-control-characters-with-datacontractjsonserializer-is-now-compatible-with-ecmascript-v6-and-v8"></a>使用 DataContractJsonSerializer 控制字符的序列化现在与 ECMAScript V6 和 V8 兼容

#### <a name="details"></a>详细信息

在 .NET Framework 4.6.2 及更低版本中，<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=fullName> 未按与 ECMAScript V6 和 V8 标准兼容的方式对一些特殊控制字符（如 \b、\f 和 \t）进行序列化。 从 .NET Framework 4.7 开始，这些控制字符的序列化与 ECMAScript V6 和 V8 兼容。

#### <a name="suggestion"></a>建议

对面向 .NET Framework 4.7 的应用，默认启用此功能。 如果不需要此行为，可以在 app.config 或 web.config 文件的 `<runtime>` 部分中添加下面的代码行，从而选择禁用此功能：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseECMAScriptV6EscapeControlCharacter=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.Xml.XmlDictionaryWriter,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject(System.Xml.XmlWriter,System.Object)?displayProperty=nameWithType>
