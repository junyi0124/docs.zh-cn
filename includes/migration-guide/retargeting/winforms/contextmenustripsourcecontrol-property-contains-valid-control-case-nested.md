---
ms.openlocfilehash: 5529b8379c5cb9f1bc525e0c2340f6b885e35822
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606223"
---
### <a name="contextmenustripsourcecontrol-property-contains-a-valid-control-in-the-case-of-nested-toolstripmenuitems"></a>存在嵌套 ToolStripMenuItems 时，ContextMenuStrip.SourceControl 属性包含有效控件

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 和更早版本中，用户从嵌套 <xref:System.Windows.Forms.ToolStripMenuItem> 控件中打开菜单时，<xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> 属性会错误地返回 null。 在 .NET Framework 4.7.2 及更高版本中，<xref:System.Windows.Forms.ContextMenuStrip.SourceControl> 属性始终设置为实际的源代码管理。

#### <a name="suggestion"></a>建议

**如何选择启用或选择弃用这些更改** 为使应用程序从这些更改获益，它必须在 .NET Framework 4.7.2 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 它面向 .NET Framework 4.7.2。 对于面向 .NET Framework 4.7.2 或更高版本的 Windows 窗体应用程序，此更改将默认启用。
- 它面向 .NET Framework 4.7.1 或更早版本，通过向 app.config 文件的 `<runtime>` 部分添加以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版辅助功能行为，如下例所示。

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue=false"/>
</runtime>
```

面向 .NET Framework 4.7.2 或更高版本并希望保留旧版行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版源代码管理值。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType>
