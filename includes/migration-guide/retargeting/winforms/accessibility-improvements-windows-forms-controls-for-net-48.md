---
ms.openlocfilehash: 882c4c0455b7df538079ffe1b7d1d7ca8af1904a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606583"
---
### <a name="accessibility-improvements-in-windows-forms-controls-for-net-48"></a>适用于 .NET 4.8 的 Windows 窗体控件中的辅助功能改进

#### <a name="details"></a>详细信息

Windows 窗体框架持续改进其与辅助功能技术的协作方式，以更好地支持 Windows 窗体客户。 其中包括以下更改：

- 改进高对比度模式的显示效果。
- 对讲述人交互的更改。
- 辅助功能层次结构中的更改（通过 UI 自动化树改进导航）。

#### <a name="suggestion"></a>建议

**如何选择加入或选择退出这些更改** 为使应用程序从这些更改中获益，它必须在 .NET Framework 4.8 上运行。 应用程序可通过以下任一方式选择加入这些更改：

- 重新编译为面向 .NET Framework 4.8。 对于面向 .NET Framework 4.8 的 Windows 窗体应用程序，这些辅助功能更改将默认启用。
- 它面向 .NET Framework 4.7.2 或更低版本，通过向应用配置文件的 `<runtime>` 部分添加以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版辅助功能行为，如下例所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
  </runtime>
</configuration>
```

请注意，要选择加入 .NET Framework 4.8 中添加的辅助功能，还必须选择加入 .NET Framework 4.7.1 及 4.7.2 的辅助功能。 面向 .NET Framework 4.8 且希望保留旧版辅助功能行为的应用程序可以通过将此 AppContext 开关显式设置为 `true` 来选择加入使用旧版辅助功能。启用键盘 ToolTip 调用支持需要将 `Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false` 行添加到 AppContextSwitchOverrides 值：

```xml
<AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false" />
```

请注意，启用此功能需要选择加入上述 .NET Framework 4.7.1 到 4.8 的辅助功能。 此外，如果未选择加入任何辅助功能，但选择加入了工具提示显示功能，则首次访问这些功能时将引发运行时 <xref:System.NotSupportedException>。 异常消息表明键盘工具提示需要启用第 3 级辅助功能改进。

**在高对比度主题中使用 OS 定义的颜色**

- 改进了高对比度主题。

**改进了讲述人支持**

- 在说出 <xref:System.Windows.Forms.DataGridViewCell> 的辅助功能名称时，讲述人现在会说出 <xref:System.Windows.Forms.DataGridViewColumn> 的排序方向。

**改进了 CheckedListBox 辅助功能支持**

- 改进了 <xref:System.Windows.Forms.CheckedListBox> 控件的讲述人支持。 使用键盘导航到 <xref:System.Windows.Forms.CheckedListBox> 控件时，讲述人会聚焦 <xref:System.Windows.Forms.CheckedListBox> 项并说出该项。
- 控件变得聚焦时，空的 CheckedListBox 控件现包含为第一个虚拟项绘制的焦点矩形。

**改进了 ComboBox 辅助功能支持**

- 为 <xref:System.Windows.Forms.ComboBox> 控件启用了 UI 自动化支持，并且可以使用 UI 自动化通知和其他 UI 自动化功能。
**改进了 DataGridView 辅助功能支持**

- 为 <xref:System.Windows.Forms.DataGridView> 控件启用了 UI 自动化支持，并且可以使用 UI 自动化通知和其他 UI 自动化功能。
- 对应于 <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl> 或 <xref:System.Windows.Forms.DataGridViewTextBoxEditingControl> 的 UI 自动化元素现在是相应编辑单元的子元素。

**改进了 LinkLabel 辅助功能支持**

- 改进了 <xref:System.Windows.Forms.LinkLabel> 控件辅助功能：如果禁用了对应的 <xref:System.Windows.Forms.LinkLabel> 控件，讲述人会指示链接的禁用状态。

**改进了 ProgressBar 辅助功能支持**

- 为 <xref:System.Windows.Forms.ProgressBar> 控件启用了 UI 自动化支持，并且可以使用 UI 自动化通知和其他 UI 自动化功能。 开发人员现可使用 UI 自动化通知，讲述人可以通过这些通知来指示进度。
有关 UI 自动化事件概述（包括 UI 自动化通知事件）的概述，请参阅 [UI 自动化事件概述](/windows/desktop/WinAuto/uiauto-eventsoverview)。

**改进了 PropertyGrid 辅助功能支持**

- 为 <xref:System.Windows.Forms.PropertyGrid> 控件启用了 UI 自动化支持，并且可以使用 UI 自动化通知和其他 UI 自动化功能。
- 与当前编辑的属性对应的 UI 自动化元素现在是对应属性项 UI 自动化元素的子元素。
- 如果父 <xref:System.Windows.Forms.PropertyGrid> 控件设置为类别视图，则 UI 自动化属性项元素现在是对应类别元素的子元素。

**改进了 ToolStrip 支持**

- 为 <xref:System.Windows.Forms.ToolStrip> 控件启用了 UI 自动化支持，并且可以使用 UI 自动化通知和其他 UI 自动化功能。
- 改进了对 <xref:System.Windows.Forms.ToolStrip> 项的导航。
- 在项模式下，讲述人焦点不会消失，也不会转到隐藏项。

**改进了视觉提示**

- 空的 <xref:System.Windows.Forms.CheckedListBox> 控件现在在接收焦点时显示焦点指示器。
注意：在运行时为控件启用了 UI 自动化支持，但设计时未使用该支持。 有关 UI 自动化的概述，请参阅 [UI 自动化概述](../../../../docs/framework/ui-automation/ui-automation-overview.md)。

**通过键盘调用控件的工具提示**

- 现可通过使用键盘聚焦控件来调用控件工具提示。 应用程序必须显式启用此功能（请参阅 **&quot;如何选择加入或退出这些更改&quot;** 部分）

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.8         |
| 类型    | 重定目标 |
