---
ms.openlocfilehash: fa24c664e9f7cf6da78d0703c7ebb52c8ebbec20
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606364"
---
### <a name="accessibility-improvements-in-windows-forms-controls"></a>Windows 窗体控件中的辅助功能改进

#### <a name="details"></a>详细信息

Windows 窗体正在使用辅助功能技术改进工作方式，以更好地支持 Windows 窗体客户。 包括从 .NET Framework 4.7.1 开始的以下改进：

- 改进高对比度模式的显示效果。
- 提升属性浏览器体验。 属性浏览器改进包括：
- 更好地通过各种下拉选择窗口使用键盘导航。
- 减少不必要的制表位。
- 更好地报告控件类型。
- 改进了讲述人行为。
- 在控件中实现缺少 UI 辅助功能模式。

#### <a name="suggestion"></a>建议

**如何选择启用或选择弃用这些更改** 为使应用程序从这些更改获益，它必须在 .NET Framework 4.7.1 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 重新编译为面向 .NET Framework 4.7.1。 对于面向 .NET Framework 4.7.1 或更高版本的 Windows 窗体 应用程序，这些辅助功能默认情况下处于启用状态。
- 通过向 app config 文件的 `<runtime>` 部分添加以下 [AppContext 交换机](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择退出旧版辅助功能行为，如下例所示。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
  </startup>
  <runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
  </runtime>
</configuration>
```

面向 .NET Framework 4.7.1 或更高版本并希望保留旧版辅助功能行为的应用程序，可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版辅助功能。<p/>有关 UI 自动化的概述，请参阅 [UI 自动化概述](~/docs/framework/ui-automation/ui-automation-overview.md)。<p/>**为 UI 自动化模式和属性添加的支持**<br/>辅助功能客户端可通过使用常用的公开描述调用模式来利用新的 WinForms 辅助功能。 这些模式并非特定于 WinForms。 例如，辅助功能客户端可以在 IAccessible 接口 (MAAS) 上调用 QueryInterface 方法，来获取 IServiceProvider 接口。 如果该接口可用，则客户端可以使用其 QueryService 方法来请求 IAccessibleEx 接口。 有关详细信息，请参阅[从客户端使用 IAccessibleEx](/windows/desktop/WinAuto/using-iaccessibleex-from-a-client)。 从 .NET Framework 4.7.1 开始，IServiceProvider 和 [IAccessibleEx](/windows/desktop/WinAuto/iaccessibleex)（在适用情况下）可用于 WinForms 辅助功能对象。<p/>.NET Framework 4.7.1 添加了对以下 UI 自动化模式和属性的支持：

- <xref:System.Windows.Forms.ToolStripSplitButton> 和 <xref:System.Windows.Forms.ComboBox> 控件支持[展开/折叠模式](~/docs/framework/ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)。
- <xref:System.Windows.Forms.ToolStripMenuItem> 控件具有 [ControlType](~/docs/framework/ui-automation/ui-automation-support-for-the-menubar-control-type.md) 属性值 <xref:System.Windows.Automation.ControlType.MenuItem?displayProperty=nameWithType>。
- <xref:System.Windows.Forms.ToolStripItem> 控件支持 <xref:System.Windows.Automation.AutomationElement.NameProperty> 属性和[展开/折叠模式](~/docs/framework/ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)。
- <xref:System.Windows.Forms.ToolStripDropDownItem> 控件支持 <xref:System.Windows.Forms.AccessibleEvents> 在下拉列表展开或折叠时指示 StateChange 和 NameChange。
- <xref:System.Windows.Forms.ToolStripDropDownButton> 控件具有 <xref:System.Windows.Automation.ControlType.MenuItem?displayProperty=nameWithType> 的 [ControlType](~/docs/framework/ui-automation/ui-automation-support-for-the-menubar-control-type.md) 属性值。
- <xref:System.Windows.Forms.DataGridViewCheckBoxCell> 控件支持 <xref:System.Windows.Automation.TogglePattern>。
- <xref:System.Windows.Forms.NumericUpDown> 和 <xref:System.Windows.Forms.DomainUpDown> 控件支持 <xref:System.Windows.Automation.AutomationElement.NameProperty> 属性并具有 <xref:System.Windows.Automation.ControlType.Spinner?displayProperty=nameWithType> 的 [ControlType](~/docs/framework/ui-automation/ui-automation-support-for-the-spinner-control-type.md)。</p>
**对 PropertyGrid 控件的改进** .NET Framework 4.7.1 向 PropertyBrowser 控件添加了以下改进：

- 用户在 <xref:System.Windows.Forms.PropertyGrid> 控件中输入错误值时所显示错误对话框中的“详细信息”按钮支持[展开/折叠模式](~/docs/framework/ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)、状态和名称更改通知以及带有 <xref:System.Windows.Automation.ControlType.MenuItem?displayProperty=nameWithType> 值的 [ControlType](~/docs/framework/ui-automation/ui-automation-support-for-the-menubar-control-type.md) 属性。
- 展开错误对话框的“详细信息”按钮时显示的信息窗格，现在可以通过键盘访问并允许讲述人宣布错误信息内容。
- <xref:System.Windows.Forms.PropertyGrid> 控件中的 <xref:System.Windows.Forms.AccessibleRole> 行已从&quot;行&quot;更改为&quot;单元格&quot;。 映射到 UIA ControlType &quot;DataItem&quot; 的单元，使其支持相应的键盘快捷方式和讲述人朗读。
- <xref:System.Windows.Forms.PropertyGrid> 控件行，当 <xref:System.Windows.Forms.PropertyGrid> 控件设置为 <xref:System.Windows.Forms.PropertySort.Categorized?displayProperty=nameWithType> 的 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性具有 <xref:System.Windows.Automation.ControlType.Button?displayProperty=nameWithType> 的 [ControlType](~/docs/framework/ui-automation/ui-automation-support-for-the-menubar-control-type.md) 属性值时，它代表标头项。
- <xref:System.Windows.Forms.PropertyGrid> 控件行，当 <xref:System.Windows.Forms.PropertyGrid> 控件设置为 <xref:System.Windows.Forms.PropertySort.Categorized?displayProperty=nameWithType> 的 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性支持[展开/折叠模式](~/docs/framework/ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)时，它代表标头项。
- 改进了网格与其上工具栏之间的键盘导航。 ,现在按 &quot;Shift-Tab&quot; 会选中第一个工具栏按钮而不是整个工具栏。
- 在高对比度模式中显示的 <xref:System.Windows.Forms.PropertyGrid> 控件现在将围绕与当前 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性值相对应的工具栏按钮绘制焦点矩形。
- 在高对比度模式中显示 <xref:System.Windows.Forms.PropertyGrid> 控件且 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性设置为 <xref:System.Windows.Forms.PropertySort.Categorized?displayProperty=nameWithType>，现在将以高对比度颜色显示类别标题的背景。
- <xref:System.Windows.Forms.PropertyGrid> 控件更好地区分具有焦点的工具栏项和指示 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性当前值的工具栏项。 此修补程序包含高对比度更改和非高对比度方案的更改。
- 指示 <xref:System.Windows.Forms.PropertyGrid.PropertySort> 属性当前值的 <xref:System.Windows.Forms.PropertyGrid> 控件工具栏项支持 <xref:System.Windows.Automation.TogglePattern>。
- 改进的讲述人支持在对齐方式选择器中区分所选对齐。
- 当窗体上显示空 <xref:System.Windows.Forms.PropertyGrid> 控件时，现可将焦点置于其上，而之前不能。

**在高对比度主题中使用 OS 定义的颜色**

- <xref:System.Windows.Forms.Button> 和 <xref:System.Windows.Forms.CheckBox> 控件的 <xref:System.Windows.Forms.ButtonBase.FlatStyle> 属性设置为 <xref:System.Windows.Forms.FlatStyle.System?displayProperty=nameWithType>（默认样式），现在，在高对比度主题中选中这两个控件时，它们会使用 OS 定义的颜色。 以前，文本和背景颜色对比度低，难以阅读。
- <xref:System.Windows.Forms.Button>、<xref:System.Windows.Forms.CheckBox>、<xref:System.Windows.Forms.RadioButton>、<xref:System.Windows.Forms.Label>、<xref:System.Windows.Forms.LinkLabel> 和 <xref:System.Windows.Forms.GroupBox> 控件及其设置为“false”的 <xref:System.Windows.Forms.Control.Enabled> 属性在高对比度主题中使用阴影颜色呈现文本，从而降低与背景的对比度。 现在这些控件使用 OS 定义的&quot;无效文本&quot;颜色。 此修补程序适用于将 `FlatStyle` 属性设置为某个值而不是 <xref:System.Windows.Forms.FlatStyle.System?displayProperty=nameWithType> 的控件。 后一种控件由 OS 呈现。
- <xref:System.Windows.Forms.DataGridView> 现在围绕具有当前焦点的单元内容呈现可见的矩形。 以前，在某些高对比度主题中这是不可见的。
- 将 <xref:System.Windows.Forms.ToolStripMenuItem.Enabled> 属性设置为“false”的 <xref:System.Windows.Forms.ToolStripMenuItem> 控件现在使用 OS 定义的&quot;无效文本&quot;颜色。
- 将 <xref:System.Windows.Forms.ToolStripMenuItem.Checked> 属性设置为“true”的 <xref:System.Windows.Forms.ToolStripMenuItem> 控件现以系统颜色的对比色呈现相关复选标记。  以前，复选标记颜色的对比度不足，在高对比度主题中不可见。
注意：Windows 10 已更改部分高对比度系统颜色的值。 Windows 窗体框架基于 Win32 框架。 为获得最佳体验，请运行最新版本的 Windows，并通过在测试应用程序中添加 app.manifest 文件选择使用最新的 OS 更改，同时取消注释以下代码：

```xml
<!-- Windows 10 -->
<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
```

**改进了的键盘导航**

- 当 <xref:System.Windows.Forms.ComboBox> 控件将其 <xref:System.Windows.Forms.ComboBox.DropDownStyle> 属性设置为 <xref:System.Windows.Forms.ComboBoxStyle.DropDownList?displayProperty=nameWithType> 并且为窗体上 Tab 键顺序中的第一个控件时，在使用键盘打开父级窗体时，该控件现在将显示焦点矩形。 在此更改之前，键盘焦点在控件上，但不会呈现焦点指示器。

**改进了讲述人支持**

- <xref:System.Windows.Forms.MonthCalendar> 控件对访问控件添加了辅助技术支持，包括使讲述人朗读控件值的功能（以前无法读取）。

- 当 <xref:System.Windows.Forms.CheckBox.CheckState?displayProperty=nameWithType> 属性发生更改时，<xref:System.Windows.Forms.CheckedListBox> 控件现在会通知讲述人。 以前，讲述人不会收到通知，因此在 <xref:System.Windows.Forms.CheckBox.CheckState> 属性更新时，用户也不会收到通知。
- <xref:System.Windows.Forms.LinkLabel> 控件更改了通知讲述人控件文本的方式。 以前，讲述人公布此文本两次并将 &quot;&amp;&quot; 符号作为实际文本进行朗读，即使这些符号对用户不可见。 已从讲述人朗读中删除重复的文本以及不必要的 &quot;&amp;&quot; 符号。
- <xref:System.Windows.Forms.DataGridViewCell> 控件类型现在正确地向讲述人和其他辅助技术报告只读状态。
- 讲述人现在可在 [多文档界面]~/docs/framework/winforms/advanced/multiple-document-interface-mdi-applications.md) 应用程序中读取子窗口的系统菜单。
- 讲述人现在能朗读将 <xref:System.Windows.Forms.ToolStripItem.Enabled?displayProperty=nameWithType> 属性设置为 **false** 的 <xref:System.Windows.Forms.ToolStripMenuItem> 控件。 以前，讲述人无法将焦点置于已禁用的菜单项来朗读内容。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.8         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Forms.ToolStripDropDownButton.CreateAccessibilityInstance?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.DomainUpDownAccessibleObject.Name?displayProperty=nameWithType>
- [MonthCalendar.AccessibilityObject](xref:System.Windows.Forms.Control.AccessibilityObject)
