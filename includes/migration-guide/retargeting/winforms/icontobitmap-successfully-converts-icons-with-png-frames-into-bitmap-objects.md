---
ms.openlocfilehash: 811b1a91169eeebfa056d9ecdb07d342d3b32d85
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615613"
---
### <a name="icontobitmap-successfully-converts-icons-with-png-frames-into-bitmap-objects"></a>Icon.ToBitmap 成功将带 PNG 帧的图标转换为位图对象

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.6 的应用开始，<xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> 方法成功将带 PNG 帧的图标转换为 Bitmap 对象。<p/>在面向 .NET Framework 4.5.2 和更早版本的应用中，如果 Icon 对象具有 PNG 帧，<xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> 方法将引发 <xref:System.ArgumentOutOfRangeException> 异常。<p/>此更改会影响以下应用：重新编译为面向 .NET Framework 4.6 的应用，以及对在 Icon 对象具有 PNG 帧时引发的 <xref:System.ArgumentOutOfRangeException> 实施特殊处理的应用。 在.NET Framework 4.6 下运行时，转换成功，不再引发 <xref:System.ArgumentOutOfRangeException> ，因此不再调用异常处理程序。

#### <a name="suggestion"></a>建议

如果不需要此行为，可以在 app.config 文件的 `<runtime>` 部分中添加下面的元素，从而保留旧行为：

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Drawing.DontSupportPngFramesInIcons=true&quot; /&gt;&#13;&#10;</code></pre>

如果 app.config 文件中已包含 `AppContextSwitchOverrides` 元素，则新值应与值特性合并，如下所示：

<pre><code class="lang-xml">&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Drawing.DontSupportPngFramesInIcons=true;&lt;previous key&gt;=&lt;previous value&gt;&quot; /&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Drawing.Icon.ToBitmap?displayProperty=nameWithType>
