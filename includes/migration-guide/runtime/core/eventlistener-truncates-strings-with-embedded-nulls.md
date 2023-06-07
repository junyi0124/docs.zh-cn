---
ms.openlocfilehash: e47a24239872e3fe86ff6902f66b38daaa106598
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497313"
---
### <a name="eventlistener-truncates-strings-with-embedded-nulls"></a>EventListener 将截断带有嵌入 NULL 的字符串

#### <a name="details"></a>详细信息

<xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> 将截断带有嵌入的 null 的字符串。 Null 字符不受 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 类支持。 此更改仅影响使用 <xref:System.Diagnostics.Tracing.EventListener?displayProperty=fullName> 读取进程中 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 数据的应用以及使用 null 字符串作为分隔符的应用。

#### <a name="suggestion"></a>建议

应可能更新 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 数据，以便不使用嵌入的空字符。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Diagnostics.Tracing.EventListener.%23ctor>
- <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType>
- <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords)?displayProperty=nameWithType>
- <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords,System.Collections.Generic.IDictionary{System.String,System.String})?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Diagnostics.Tracing.EventListener.#ctor`
- `M:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)`
- `M:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords)`
- `M:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel,System.Diagnostics.Tracing.EventKeywords,System.Collections.Generic.IDictionary{System.String,System.String})`

-->
