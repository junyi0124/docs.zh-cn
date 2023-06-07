---
ms.openlocfilehash: 5a96b40e5e0df6a47415acecab410444a713632b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496711"
---
### <a name="etw-eventlisteners-do-not-capture-events-from-providers-with-explicit-keywords-like-the-tpl-provider"></a>ETW EventListeners 无法从具有显式关键字的提供程序中（例如 TPL 提供程序）捕获事件

#### <a name="details"></a>详细信息

具有空白关键字掩码的 ETW EventListeners 无法从具有显式关键字的提供程序中正确捕获事件。 在 .NET Framework 4.5 中，TPL 提供程序开始提供显式关键字，引发了此问题。 在 .NET Framework 4.6 中，EventListeners 已更新，此问题不再存在。

#### <a name="suggestion"></a>建议

要解决此问题，请将对 <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)> 的调用替换为 EnableEvents 重载，以显式指定要使用的“任意关键字”掩码：<code>EnableEvents(eventSource, level, unchecked((EventKeywords)0xFFFFffffFFFFffff))</code>。此外，此问题已在 .NET Framework 4.6 中解决，因此升级到该版本的 .NET Framework 即可解决该问题。

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)`

-->
