---
ms.openlocfilehash: aadf5eb85c8736c29639d49bc8baf21545d2467c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606927"
---
### <a name="net-com-successfully-marshals-byref-safearray-parameters-on-events"></a>.NET COM 成功封送事件上的 ByRef SafeArray 参数

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.2 及更低版本中，COM 事件上的 ByRef [SafeArray](/windows/desktop/api/oaidl/ns-oaidl-safearray) 参数无法封送回本机代码。  进行此更改后，[SafeArray](/windows/desktop/api/oaidl/ns-oaidl-safearray) 现在可成功进行封送。<ul><li>[x] Quirked</li></ul>

#### <a name="suggestion"></a>建议

如果正确封送 COM 事件上的 ByRef SafeArray 参数会中断执行，则可以通过将以下配置开关添加到应用程序配置来禁用此代码：<pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;Switch.System.Runtime.InteropServices.DoNotMarshalOutByrefSafeArrayOnInvoke&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
