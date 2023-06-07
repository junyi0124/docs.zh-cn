---
ms.openlocfilehash: dfa8235fcfd868b80d3a610bddb492899519e289
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009043"
---
### <a name="xml-parsing-changes"></a>XML 分析更改

| 名称    | 值   |
|:--------|:--------|
| 范围   | 次要   |
| Version | 4.5.2   |
| 类型    | 运行时 |

#### <a name="details"></a>详细信息

出于安全原因，XML 分析 API 中引入了以下更改：

- 初始化 <xref:System.Xml.XmlReaderSettings> 时，<xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities?displayProperty=nameWithType> 设置为 1 千万。
- 默认情况下，<xref:System.Xml.XmlReaderSettings.XmlResolver?displayProperty=nameWithType> 设置为 `null`。

> [!NOTE]
> 所有 XML 分析器都使用 <xref:System.Xml.XmlReaderSettings>，因此尽管此更改有助于 <xref:System.Xml.XmlReader> 的情况，但它也会影响其他情况。

#### <a name="suggestion"></a>建议

若要恢复到以前的行为，可以在注册表中设置一个值。 将名为 `EnableLegacyXmlSettings` 的 DWORD 值添加到 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\XML` 注册表项，并将其值设置为 `1`。 还可以改为在 HKEY_CURRENT_USER 配置单元中添加注册表值。

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Xml.XmlReaderSettings.MaxCharactersFromEntities?displayProperty=fullName>
- <xref:System.Xml.XmlReaderSettings.XmlResolver?displayProperty=fullName>

此外，任何直接或间接依赖于 <xref:System.Xml.XmlResolver> 的 XML API 都会受到影响。

<!--

#### Affected APIs

- `P:System.Xml.XmlReaderSettings.MaxCharactersFromEntities`
- `P:System.Xml.XmlReaderSettings.XmlResolver`

-->
