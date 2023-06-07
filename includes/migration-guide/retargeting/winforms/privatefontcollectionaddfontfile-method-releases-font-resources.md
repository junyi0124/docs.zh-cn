---
ms.openlocfilehash: 6ee290f6722480778381376f588e16877894f232
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606199"
---
### <a name="privatefontcollectionaddfontfile-method-releases-font-resources"></a>PrivateFontCollection.AddFontFile 方法释放字体资源

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 和早期版本中，为使用 <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)> 方法添加到此集合的 <xref:System.Drawing.Font> 对象设置 <xref:System.Drawing.Text.PrivateFontCollection> 后，<xref:System.Drawing.Text.PrivateFontCollection?displayProperty=nameWithType> 类不会释放 GDI+ 字体资源。 在 .NET Framework 4.7.2 和更高版本中，<xref:System.Drawing.Text.FontCollection.Dispose%2A> 会释放作为文件添加到此集合的 GDI+ 字体。

#### <a name="suggestion"></a>建议

**如何选择启用或选择弃用这些更改** 为使应用程序从这些更改获益，它必须在 .NET Framework 4.7.2 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 重新编译为面向 .NET Framework 4.7.2。 对于面向 .NET Framework 4.7.2 或更高版本的 Windows 窗体应用程序，此更改将默认启用。
- 它面向 .NET Framework 4.7.1 或更早版本，通过向 app.config 文件的 `<runtime>` 部分添加以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版辅助功能行为，如下例所示。

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Drawing.Text.DoNotRemoveGdiFontsResourcesFromFontCollection=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

面向 .NET Framework 4.7.2 或更高版本并希望保留旧版行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择不释放字体资源。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)?displayProperty=nameWithType>
- <xref:System.Drawing.Text.FontCollection.Dispose?displayProperty=nameWithType>
