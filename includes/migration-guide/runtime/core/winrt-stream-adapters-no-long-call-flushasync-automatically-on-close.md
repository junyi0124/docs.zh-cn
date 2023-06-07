---
ms.openlocfilehash: 2b4f35fe15f806897e5e4d85ee774b2e4572d974
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496207"
---
### <a name="winrt-stream-adapters-no-long-call-flushasync-automatically-on-close"></a>WinRT 流适配器不再在关闭时自动调用 FlushAsync

#### <a name="details"></a>详细信息

在 Windows 应用商店的应用中，Windows 运行时流适配器不再从 Dispose 方法调用 FlushAsync 方法。

#### <a name="suggestion"></a>建议

此更改应该为透明。 开发人员可以通过编写如下代码还原之前的行为：<pre><code class="lang-csharp">using (var stream = GetWindowsRuntimeStream() as Stream)&#13;&#10;{&#13;&#10;// do something&#13;&#10;await stream.FlushAsync();&#13;&#10;}&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |透明|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
