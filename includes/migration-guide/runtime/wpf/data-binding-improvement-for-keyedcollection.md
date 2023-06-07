---
ms.openlocfilehash: 9cd1d0955b8b77cb77d5c6938b37d9042d8144f6
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606813"
---
### <a name="data-binding-improvement-for-keyedcollection"></a>KeyedCollection 的数据绑定改进

#### <a name="details"></a>详细信息

修复了源对象使用相同签名（例如，KeyedCollection&lt;int,TItem&gt;）声明自定义索引器时，<xref:System.Windows.Data.Binding> 错误使用 IList 索引器的问题。

#### <a name="suggestion"></a>建议

为了使针对较旧版本的应用程序能够从此更改中受益，它必须在 .NET Framework 4.8 或更高版本上运行，并且必须通过将以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)添加到应用配置文件的 <code>&lt;runtime&gt;</code> 部分并将其设置为 <code>false</code> 来选择加入更改：<pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;startup&gt;&#13;&#10;&lt;supportedRuntime version=&quot;v4.0&quot; sku=&quot;.NETFramework,Version=v4.7&quot;/&gt;&#13;&#10;&lt;/startup&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Data.Binding.IListIndexerHidesCustomIndexer=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
