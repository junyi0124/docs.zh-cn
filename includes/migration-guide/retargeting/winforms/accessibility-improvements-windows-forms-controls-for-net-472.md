---
ms.openlocfilehash: a21824862d6cad046b5d6186f9d6db9c20438304
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606347"
---
### <a name="accessibility-improvements-in-windows-forms-controls-for-net-472"></a>适用于 .NET 4.7.2 的 Windows 窗体控件中的辅助功能改进

#### <a name="details"></a>详细信息

Windows 窗体框架正在改进其辅助功能技术的工作方式，以更好地支持 Windows 窗体客户。 其中包括以下更改：

- 改进高对比度模式的显示效果。
- 用于改进 DataGridView 和 MenuStrip 控件中的键盘导航的更改。
- 对讲述人交互的更改。

#### <a name="suggestion"></a>建议

**如何选择启用或选择弃用这些更改** 为使应用程序从这些更改获益，它必须在 .NET Framework 4.7.2 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 重新编译为面向 .NET Framework 4.7.2。 对于面向 .NET Framework 4.7.2 或更高版本的 Windows 窗体 应用程序，这些辅助功能更改将默认启用。
- 它面向 .NET Framework 4.7.1 或更早版本，通过向 app config 文件的 `<runtime>` 部分添加以下 [AppContext 开关](../../../../docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版辅助功能行为，如下例所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
  </runtime>
</configuration>
```

请注意，要选择启用 .NET Framework 4.7.2 中添加的辅助功能，还必须选择启用 .NET Framework 4.7.1 的辅助功能。 面向 .NET Framework 4.7.2 或更高版本并希望保留旧版辅助功能行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版辅助功能。

**在高对比度主题中使用 OS 定义的颜色**

- <xref:System.Windows.Forms.ToolStripDropDownButton> 的下拉菜单现在在高对比度主题中使用 OS 定义的颜色。
- 现在，在高对比度主题中选中将 <xref:System.Windows.Forms.Button>、<xref:System.Windows.Forms.RadioButton> 和 <xref:System.Windows.Forms.CheckBox> 控件的 <xref:System.Windows.Forms.ButtonBase.FlatStyle> 设置为 <xref:System.Windows.Forms.FlatStyle.Flat?displayProperty=nameWithType> 或 <xref:System.Windows.Forms.FlatStyle.Popup?displayProperty=nameWithType> 时，它们会使用 OS 定义的颜色。 以前，文本和背景颜色对比度低，难以阅读。
- 包含在 <xref:System.Windows.Forms.GroupBox>（其 <xref:System.Windows.Forms.Control.Enabled> 属性设置为 `false`）中的控件现在将在高对比度主题中使用 OS 定义的颜色。
- 在高对比度模式下，<xref:System.Windows.Forms.ToolStripButton>、<xref:System.Windows.Forms.ToolStripComboBox> 和 <xref:System.Windows.Forms.ToolStripDropDownButton> 控件的亮度对比度提高。
- 对于 <xref:System.Windows.Forms.DataGridViewLinkCell.LinkColor?displayProperty=nameWithType> 属性，<xref:System.Windows.Forms.DataGridViewLinkCell> 将默认使用高对比模式下 OS 定义的颜色。
注意：Windows 10 已更改部分高对比度系统颜色的值。 Windows 窗体框架基于 Win32 框架。 为获得最佳体验，请运行最新版本的 Windows，并通过在测试应用程序中添加 app.manifest 文件选择使用最新的 OS 更改，同时取消注释以下代码：

```xml
<!-- Windows 10 -->
<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
```

**改进了讲述人支持**

- 现在，讲述人将在公布 <xref:System.Windows.Forms.ToolStripMenuItem> 的文本时公布 <xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys?displayProperty=nameWithType> 属性的值。
- 当 <xref:System.Windows.Forms.ToolStripMenuItem> 的 <xref:System.Windows.Forms.Control.Enabled> 属性设置为 `false` 时，讲述人现在会有所指示。
- 当 <xref:System.Windows.Forms.ListView.CheckBoxes?displayProperty=nameWithType> 属性设置为 `true` 时，讲述人现在会针对复选框的状态给出反馈。
- 讲述人扫描模式焦点顺序现在与 ClickOnce 下载对话框窗口中控件的视觉顺序一致。

**改进了 DataGridView 辅助功能支持**

- <xref:System.Windows.Forms.DataGridView> 中的行现在可使用键盘进行排序。 现在用户可使用 F3 键按当前列排序。
- 若 <xref:System.Windows.Forms.DataGridView.SelectionMode?displayProperty=nameWithType> 设置为 <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType>，当用户按 Tab 键遍历当前行中的单元格时，列标题将更改颜色来指示当前列。
- <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Parent?displayProperty=nameWithType> 属性现在返回正确的父控件。

**改进了视觉提示**

- 具有空 <xref:System.Windows.Forms.ButtonBase.Text> 属性的 <xref:System.Windows.Forms.RadioButton> 和 <xref:System.Windows.Forms.CheckBox> 控件现在将在接收到焦点时显示焦点指示器。

**改进了属性网格支持**

- 现在，仅在 PropertyGrid 元素启用时，<xref:System.Windows.Forms.PropertyGrid> 控件子元素才会为 <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> 属性返回 `true`。
- 现在，仅在用户可更改 PropertyGrid 元素时，<xref:System.Windows.Forms.PropertyGrid> 控件子元素才会为 <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> 属性返回 `false`。
有关 UI 自动化的概述，请参阅 [UI 自动化概述](../../../../docs/framework/ui-automation/ui-automation-overview.md)。</p>**改进了的键盘导航**

- <xref:System.Windows.Forms.ToolStripButton> 包含在将 <xref:System.Windows.Forms.ToolStripPanel.TabStop> 属性设置为 `true` 的 <xref:System.Windows.Forms.ToolStripPanel> 中时，现在允许聚焦。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.7.2       |
| 类型    | 重定目标 |
