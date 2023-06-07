---
ms.openlocfilehash: c64431fd651fd7d53fb46231c6acc10c5cb43fff
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606936"
---
### <a name="winforms-domain-upbutton-and-downbutton-actions-are-in-sync-now"></a>WinForm 的域 upbutton 和 downbutton 操作现已同步

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 和早期版本中，显示控件文本时会忽视 <xref:System.Windows.Forms.DomainUpDown> 控件的 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 操作，并且要求开发人员先使用控件上的 <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> 操作，再使用 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 操作。 从 .NET Framework 4.7.2 开始，<xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 和 <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> 操作将在此场景中独立工作，并保持同步。

#### <a name="suggestion"></a>建议

为使应用程序从这些更改获益，它必须在 .NET Framework 4.7.2 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 重新编译为面向 .NET Framework 4.7.2。 对于面向 .NET Framework 4.7.2 或更高版本的 Windows 窗体应用程序，此更改将默认启用。
- 通过向应用配置文件的 `<runtime>` 部分添加以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版滚动行为，如下例所示。

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType>
