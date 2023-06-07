---
title: .NET Framework 中辅助功能的新增功能
description: 请参阅从 .NET Framework 4.7.1 开始的 .NET 辅助功能的新增功能。 利用辅助功能，应用可以为辅助技术用户提供正确的体验。
ms.date: 04/18/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.openlocfilehash: d204bea7f5ec1ed0c25b7b2dedd04d61c7f3e93d
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679542"
---
# <a name="whats-new-in-accessibility-in-the-net-framework"></a>.NET Framework 中辅助功能的新增功能

.NET Framework 旨在让用户更轻松地使用应用程序。 辅助功能使应用程序能够为辅助技术用户提供最佳体验。 从 .NET Framework 4.7.1 开始，.NET Framework 包括大量辅助功能改进，使开发人员能够创建易于访问的应用程序。

## <a name="accessibility-switches"></a>辅助功能开关

如果应用面向 .NET Framework 4.7 或更低版本，但是在 .NET Framework 4.7.1 或更高版本上运行，可以将应用配置为选择使用辅助功能。 如果应用面向 .NET Framework 4.7.1 或更高版本，还可以将应用配置为使用旧版功能（且不使用辅助功能）。 包括辅助功能的每个 .NET Framework 版本都有一个特定于版本的辅助开关，请将它添加到应用程序配置文件 [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 部分中的 [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素。 以下是受支持的开关：

|Version|开关|
|---|---|
|.NET Framework 4.7.1|"Switch.UseLegacyAccessibilityFeatures"|
|.NET Framework 4.7.2|"Switch.UseLegacyAccessibilityFeatures.2"|
|.NET Framework 4.8|"Switch.UseLegacyAccessibilityFeatures.3"|

### <a name="taking-advantage-of-accessibility-enhancements"></a>利用辅助功能改进

对于面向 .NET Framework 4.7.1 或更高版本的应用程序，新的辅助功能默认情况下处于启用状态。 此外，对于面向 .NET Framework 早期版本，但在 .NET Framework 4.7.1 或更高版本上运行的应用程序，可通过将开关添加到应用程序配置文件 [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 部分中的 [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素并将其值设为 `false` 来选择弃用旧辅助功能行为（因而利用辅助功能改进）。 以下代码演示如何选择使用 .NET Framework 4.7.1 中引入的辅助功能改进：

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
</runtime>
```

如果选择使用更高的 .NET Framework 版本中的辅助功能，还必须显式选择使用更低 .NET Framework 版本的功能。 通过配置应用以利用 .NET Framework 4.7.1 和 4.7.2 中的辅助功能改进需要以下 [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素：

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
</runtime>
```

通过配置应用以利用 .NET Framework 4.7.1、4.7.2 和 4.8 中的辅助功能改进需要以下 [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素：

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
</runtime>
```

### <a name="restoring-legacy-behavior"></a>还原旧行为

对于面向 .NET Framework 4.7.1 或更高版本的应用程序，可通过将开关添加到应用程序配置文件 [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 部分中的 [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 元素并将其值设为 `true` 来禁用辅助功能。 例如，以下配置选择弃用 .NET Framework 4.7.2 中引入的辅助功能：

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures.2=true" />
</runtime>
```

## <a name="whats-new-in-accessibility-in-net-framework-48"></a>.NET Framework 4.8 中辅助功能的新增功能

.NET Framework 4.8 在以下几个领域包含新增辅助功能：

- [Windows 窗体](#winforms48)

- [Windows Presentation Foundation (WPF)](#wpf48)

- [Windows Workflow Foundation (WF) 工作流设计器](#wf48)

<a name="winforms48"></a>

### <a name="windows-forms"></a>Windows 窗体

在 .NET Framework 4.8 中，Windows 窗体将对 LiveRegions 和通知事件的支持添加到多个常用控件中。 它还会在用户使用键盘导航到控件时添加工具提示支持。

**标签和 StatusStrips 中的 UIA LiveRegions 支持**

UIA LiveRegions 允许应用程序开发人员将用户工作位置之外的某个控件中的文本更改通知给屏幕阅读器。 例如，对于显示连接状态的 <xref:System.Windows.Forms.StatusStrip> 控件，这很有用。 如果连接断开且状态发生更改，开发人员可能会希望将此情况通知给屏幕阅读器。

从 .NET Framework 4.8 开始，Windows 窗体同时为 <xref:System.Windows.Forms.Label> 和 <xref:System.Windows.Forms.StatusStrip> 控件实现 UIA LiveRegions。 例如，以下代码在名为 `label1` 的 <xref:System.Windows.Forms.Label> 控件中使用 LiveRegion：

```csharp
public Form1()
{
   InitializeComponent();
   label1.AutomationLiveSetting = AutomationLiveSetting.Polite;
}

…
Label1.Text = “Ready!”;
```

无论用户在何处与应用程序交互，讲述人都会宣布“准备就绪”。

还可以将 <xref:System.Windows.Forms.UserControl> 实现为 LiveRegion：

```csharp
using System;
using System.Windows.Forms;
using System.Windows.Forms.Automation;

namespace WindowsFormsApplication
{
   public partial class UserControl1 : UserControl, IAutomationLiveRegion
   {
      public UserControl1()
      {
         InitializeComponent();
      }

      public AutomationLiveSetting AutomationLiveSetting { get; set; }
      private AutomationLiveSetting IAutomationLiveRegion.GetLiveSetting()
      {
         return this.AutomationLiveSetting;
      }

      protected override void OnTextChanged(EventArgs e)
      {
         base.OnTextChanged(e);
         AutomationNotifications.UiaRaiseLiveRegionChangedEvent(this.AccessibilityObject);
      }
   }
}
```

**UIA 通知事件**

Windows 10 Fall Creators Update 中引入的 UIA 通知事件允许应用程序引发 UIA 事件，这使得讲述人仅根据你为事件提供的文本来进行宣读，而无需在 UI 中使用相应的控件。 在某些情况下，这是一种能够显着改善应用辅助功能的简单方法。 也可以用于通知可能需要很长时间的某些进程的进度。 有关 UIA 通知事件的详细信息，请参阅[桌面应用程序是否可以利用新的 UI 通知事件？](/archive/blogs/winuiautomation/can-your-desktop-app-leverage-the-new-uia-notification-event-in-order-to-have-narrator-say-exactly-what-your-customers-need)。

下面的示例会引发[通知事件](xref:System.Windows.Forms.AccessibleObject.RaiseAutomationNotification%2A)：

```csharp
MethodInfo raiseMethod = typeof(AccessibleObject).GetMethod("RaiseAutomationNotification");
if (raiseMethod != null) {
   raiseMethod.Invoke(progressBar1.AccessibilityObject, new object[3] {/*Other*/ 4, /*All*/ 2, "The progress is 50%." });
}
```

**键盘访问工具提示**

在面向 .NET Framework 4.7.2 及更早版本的应用程序中，只能通过将鼠标指针移到控件上，才能触发控件[工具提示](xref:System.Windows.Forms.ToolTip)。 从 .NET Framework 4.8 开始，键盘用户可以通过使用 Tab 键或箭头键（具有或不具有修饰键）来聚焦控件，从而触发控件的工具提示。 要实现这一特定的辅助功能优化效果，需要额外的 [AppContext 开关](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1"/>
   </startup>
   <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Please note that disabling Switch.UseLegacyAccessibilityFeatures, Switch.UseLegacyAccessibilityFeatures.2 and Switch.UseLegacyAccessibilityFeatures.3 is required to disable Switch.System.Windows.Forms.UseLegacyToolTipDisplay -->
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false"/>
   </runtime>
</configuration>
```

下图显示用户使用键盘选中按钮时的工具提示。

![用户使用键盘导航到按钮时工具提示的屏幕截图。](./media/whats-new-in-accessibility/select-tooltip-with-keyboard.png)

<a name="wpf48"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

从 .NET Framework 4.8 开始，WPF 包括大量辅助功能改进。

**屏幕讲述人不再宣读可见性为“折叠”或“隐藏”的元素**

可见性为“折叠”或“隐藏”的元素不再被屏幕讲述人宣读。 如果向用户宣读包含可见性为 <xref:System.Windows.Visibility.Collapsed?displayProperty=nameWithType> 或 <xref:System.Windows.Visibility.Hidden?displayProperty=nameWithType> 的元素的用户界面，屏幕阅读器可能会错误呈现这些界面。 从 .NET Framework 4.8 开始，WPF 不再在 UIAutomation 树的控制视图中包含折叠或隐藏的元素，屏幕阅读器无法再宣读这些元素。

**与非基于修饰器的文本选择配合使用的 SelectionTextBrush 属性**

在 .NET Framework 4.7.2 中，WPF 添加了无需使用修饰器层即可绘制 <xref:System.Windows.Controls.TextBox> 和 <xref:System.Windows.Controls.PasswordBox> 文本选择的功能。 此方案中所选文本的前景色由 <xref:System.Windows.SystemColors.HighlightTextBrush?displayProperty=nameWithType> 决定。

.NET Framework 4.8 添加了一个新属性 `SelectionTextBrush`，允许开发人员在使用非基于修饰器的文本选择时为所选文本选择特定画笔。 此属性仅适用于 <xref:System.Windows.Controls.Primitives.TextBoxBase> 派生的控件和启用了非基于修饰器的文本选择功能的 WPF 应用程序中的 <xref:System.Windows.Controls.PasswordBox> 控件。 该属性不适用于 <xref:System.Windows.Controls.RichTextBox> 控件。 如果未启用非基于修饰器的文本选择功能，则忽略此属性。

要使用此属性，只需将其添加到 XAML 代码，并使用适当的画笔或绑定。 生成的文本选择如下所示：

![运行中应用的屏幕截图，其中选中了“Hello World”文字。](./media/whats-new-in-accessibility/selectiontextbrush-property.png)

可以结合使用 `SelectionBrush` 和 `SelectionTextBrush` 属性来生成你认为合适的任何背景色和前景色组合。

**对 UIAutomation ControllerFor 属性的支持**

UIAutomation 的 `ControllerFor` 属性返回一系列由支持该属性的自动化元素操纵的自动化元素。 此属性通常用于自动建议辅助功能。 应在自动化元素会影响应用程序 UI 或桌面的一个或多个段时使用 `ControllerFor`。 否则，很难将控制操作的影响与 UI 元素关联。 此功能增加了控件为 `ControllerFor` 属性提供值的功能。

.NET framework 4.8 添加了新的虚拟方法 <xref:System.Windows.Automation.Peers.AutomationPeer.GetControlledPeersCore?displayProperty=nameWithType?displayProperty=nameWithType>。 要为 `ControllerFor` 属性提供值，只需替代此方法并为此 <xref:System.Windows.Automation.Peers.AutomationPeer> 操作的控件返回 `List<AutomationPeer>`：

```csharp
public class AutoSuggestTextBox: TextBox
{
   protected override AutomationPeer OnCreateAutomationPeer()
   {
      return new AutoSuggestTextBoxAutomationPeer(this);
   }

   public ListBox SuggestionListBox;
}

internal class AutoSuggestTextBoxAutomationPeer : TextBoxAutomationPeer
{
   public AutoSuggestTextBoxAutomationPeer(AutoSuggestTextBox owner) : base(owner)
   {
   }

   protected override List<AutomationPeer> GetControlledPeersCore()
   {
      List<AutomationPeer> controlledPeers = new List<AutomationPeer>();
      AutoSuggestTextBox owner = Owner as AutoSuggestTextBox;
      controlledPeers.Add(UIElementAutomationPeer.CreatePeerForElement(owner.SuggestionListBox));
      return controlledPeers;
   }
}
```

**键盘访问工具提示**

在 .NET Framework 4.7.2 和更早版本中，仅当用户将鼠标光标悬停在控件上时，系统才会显示工具提示。 在 .NET Framework 4.8 中，工具提示还会在键盘聚焦时显示，也可以通过键盘快捷方式显示。

要启用此功能，应用程序需要面向 .NET Framework 4.8，或使用 `Switch.UseLegacyAccessibilityFeatures.3` 和 `Switch.UseLegacyToolTipDisplay` [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关来选择加入。 下面是一个示例应用程序配置文件：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.UseLegacyToolTipDisplay=false" />
   </runtime>
</configuration>
```

启用后，一旦控件接收到键盘焦点，包含工具提示的所有控件都会显示工具提示。 随时间推移或当键盘焦点发生变化时，可关闭工具提示。 用户还可以通过使用新的键盘快捷键 Ctrl+Shift+F10 手动关闭工具提示。 关闭工具提示后，可以使用相同的键盘快捷键再次显示。

> [!NOTE]
> <xref:System.Windows.Controls.Ribbon.Ribbon> 控件上的[功能区工具提示](xref:System.Windows.Controls.Ribbon.RibbonToolTip)不在键盘聚焦时显示；它们仅通过键盘快捷键显示。

**添加了对 SizeOfSet 和 PositionInSet UIAutomation 属性的支持**

Windows 10 引入了两个新的 UIAutomation 属性（`SizeOfSet` 和 `PositionInSet`），应用程序用其描述集合中的项数。 然后，UIAutomation 客户端应用程序（如屏幕阅读器）可以向某个应用程序查询这些属性，并宣布应用程序 UI 的准确表示形式。

从 .NET Framework 4.8 开始，WPF 向 WPF 应用程序中的 UIAutomation 公开这两个属性。 这可以通过以下两种方式实现：

- 通过使用依赖项属性。

  WPF 添加了两个新的依赖项属性：<xref:System.Windows.Automation.AutomationProperties.SizeOfSet?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.AutomationProperties.PositionInSet?displayProperty=nameWithType>。 开发人员可以使用 XAML 来设置它们的值：

  ```xaml
  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="1">Button 1</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="2">Button 2</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="3">Button 3</Button>
  ```

- 通过替代 AutomationPeer 虚拟方法。

  <xref:System.Windows.Automation.Peers.AutomationPeer.GetSizeOfSetCore> 和 <xref:System.Windows.Automation.Peers.AutomationPeer.GetPositionInSetCore> 虚拟方法已添加到 AutomationPeer 类中。 开发人员可以通过替代这些方法为 `SizeOfSet` 和 `PositionInSet` 提供值，如以下示例所示：

  ```csharp
  public class MyButtonAutomationPeer : ButtonAutomationPeer
  {
    protected override int GetSizeOfSetCore()
    {
        // Call into your own logic to provide a value for SizeOfSet
        return CalculateSizeOfSet();
    }

    protected override int GetPositionInSetCore()
    {
        // Call into your own logic to provide a value for PositionInSet
        return CalculatePositionInSet();
    }
  }
  ```

此外，开发人员无需执行其他操作，<xref:System.Windows.Controls.ItemsControl> 实例中的项会自动为这些属性提供值。 如果将 <xref:System.Windows.Controls.ItemsControl> 归组，则组的集合表示为一个集，并且每个组计为一个单独的集，该组内的每个项提供其在该组内的位置以及该组的大小。 虚拟化不会影响自动值。 即使没有实现某个项，它仍然会计入集的总大小，并影响其在同级项集中的位置。

仅当应用程序面向 .NET Framework 4.8 时，才会提供自动值。 对于面向 .NET Framework 早期版本的应用程序，可以设置 `Switch.UseLegacyAccessibilityFeatures.3` [AppContext 开关](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)，如以下 App.config 文件中所示：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
   </runtime>
</configuration>
```

<a name="wf48"></a>

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Windows Workflow Foundation (WF) 工作流设计器

.NET Framework 4.8 中的工作流设计器包含以下更改：

- 使用讲述人的用户将体验到 FlowSwitch 事例标签的改进。

- 使用讲述人的用户将体验到按钮描述的改进。

- 选择高对比度主题的用户将看到工作流设计器及其控件可见性方面的改进，例如元素间的对比度效果更好和用于焦点元素的更明显的选择框。

如果应用程序面向 .NET Framework 4.7.2 或更早版本，则可以通过在应用程序配置文件中将 `Switch.UseLegacyAccessibilityFeatures.3` [AppContext 开关](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)设置为 `false` 来选择这些更改。 有关详细信息，请参阅本文中的[利用辅助功能改进](#taking-advantage-of-accessibility-enhancements)部分。

## <a name="whats-new-in-accessibility-in-net-framework-472"></a>.NET Framework 4.7.2 中辅助功能的新增功能

.NET Framework 4.7.2 在以下几个领域包含新增辅助功能：

- [Windows 窗体](#winforms472)

- [Windows Presentation Foundation (WPF)](#wpf472)

<a name="winforms472"></a>

### <a name="windows-forms"></a>Windows 窗体

**高对比度主题中的 OS 定义的颜色**

自 .NET Framework 4.7.2 起，Windows 窗体使用高对比度主题中的操作系统定义的颜色。 这会影响以下控件：

- <xref:System.Windows.Forms.ToolStripDropDownButton> 控件的下拉箭头。

- <xref:System.Windows.Forms.ButtonBase.FlatStyle> 设为 <xref:System.Windows.Forms.FlatStyle.Flat?displayProperty=nameWithType> 或 <xref:System.Windows.Forms.FlatStyle.Popup?displayProperty=nameWithType> 的 <xref:System.Windows.Forms.Button>、<xref:System.Windows.Forms.RadioButton> 和 <xref:System.Windows.Forms.CheckBox> 控件。 以前，所选文本和背景颜色对比度低，难以阅读。

- <xref:System.Windows.Forms.Control.Enabled> 属性设为 `false` 的 <xref:System.Windows.Forms.GroupBox> 中所包含的控件。

- 在高对比度模式下，<xref:System.Windows.Forms.ToolStripButton>、<xref:System.Windows.Forms.ToolStripComboBox> 和 <xref:System.Windows.Forms.ToolStripDropDownButton> 控件的亮度对比度提高。

- <xref:System.Windows.Forms.DataGridViewLinkCell> 的 <xref:System.Windows.Forms.DataGridViewLinkCell.LinkColor> 属性。

**讲述人改进**

自 .NET Framework 4.7.2 起，讲述人支持在以下几个方面改进：

- 它在公布 <xref:System.Windows.Forms.ToolStripMenuItem> 的文本时会公布 <xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys?displayProperty=nameWithType> 属性的值。

- 当 <xref:System.Windows.Forms.ToolStripMenuItem> 的 <xref:System.Windows.Forms.Control.Enabled> 属性设置为 `false` 时，它会有所指示。

- 当 <xref:System.Windows.Forms.ListView.CheckBoxes?displayProperty=nameWithType> 属性设置为 `true` 时，它会针对复选框的状态给出反馈。

- 讲述人扫描模式焦点顺序与 ClickOnce 下载对话框窗口中控件的视觉顺序一致。

**DataGridView 改进**

自 .NET Framework 4.7.2 起，<xref:System.Windows.Forms.DataGridView> 控件引入了以下辅助功能改进：

- 可以使用键盘对行进行排序。 用户可使用 F3 按当前列排序。

- 若 <xref:System.Windows.Forms.DataGridView.SelectionMode?displayProperty=nameWithType> 设置为 <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType>，当用户按 Tab 键遍历当前行中的单元格时，列标题将更改颜色来指示当前列。

- <xref:System.Windows.Forms.DataGridViewLinkCell.DataGridViewLinkCellAccessibleObject?displayProperty=nameWithType> 的 <xref:System.Windows.Forms.AccessibleObject.Parent?displayProperty=nameWithType> 属性返回正确的父控件。

**改进了视觉提示**

- 具有空 <xref:System.Windows.Forms.ButtonBase.Text> 属性的 <xref:System.Windows.Forms.RadioButton> 和 <xref:System.Windows.Forms.CheckBox> 控件在接收到焦点时会显示焦点指示器。

**改进了属性网格支持**

- 现在，仅在 PropertyGrid 元素启用时，<xref:System.Windows.Forms.PropertyGrid> 控件子元素才会为 <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> 属性返回 `true`。

- 仅在用户可更改 PropertyGrid 元素时，<xref:System.Windows.Forms.PropertyGrid> 控件子元素才会为 <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> 属性返回 `false`。

**改进了的键盘导航**

- <xref:System.Windows.Forms.ToolStripButton> 控件允许在焦点包含在 <xref:System.Windows.Forms.ToolStripPanel>（其 <xref:System.Windows.Forms.ToolStripPanel.TabStop> 属性设置为 `true`）中时进行聚焦

<a name="wpf472"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**对复选框和单选按钮控件的更改**

在 .NET Framework 4.7.1 和更低版本中，WPF <xref:System.Windows.Controls.CheckBox?displayProperty=nameWithType> 和 <xref:System.Windows.Controls.RadioButton?displayProperty=nameWithType> 控件不一致且在经典和高对比度主题中具有不正确的焦点视觉对象。  控件没有内容集时会出现这些问题。  这会使得主题间的转换变得混乱且难以看到焦点视觉对象。

现在，在 .NET Framework 4.7.2 中，主题间的这些视觉对象更加一致，并且在经典和高对比度主题中更轻松可见。

**在 WPF 应用程序中托管的 WinForms 控件**

对于 .NET Framework 4.7.1 和更低版本中的 WPF 应用程序托管的 WinForms 控件，如果该层中的第一个或最后一个控件是 WPF <xref:System.Windows.Forms.Integration.ElementHost> 控件，那么用户不能按 Tab 退出 WinForms 层。 现在，在 .NET Framework 4.7.2 中，用户能够按 Tab 退出 WinForms 层。

但是，依赖于永不转义 WinForms 层的焦点的自动化应用程序不再会按预期工作。

## <a name="whats-new-in-accessibility-in-net-framework-471"></a>.NET Framework 4.7.1 中辅助功能的新增功能

.NET Framework 4.7.1 在以下几个领域包含新增辅助功能：

- [Windows Presentation Foundation (WPF)](#wpf471)

- [Windows 窗体](#winforms471)

- [ASP.NET Web 控件](#aspnet471)

- [.NET SDK 工具](#tools471)

- [Windows Workflow Foundation (WF) 工作流设计器](#wf471)

<a name="wpf471"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**屏幕阅读器改进**

如果启用了辅助功能改进，.NET Framework 4.7.1 包括以下可影响屏幕阅读器的增强功能：

- 在 .NET Framework 4.7 及更低版本中，<xref:System.Windows.Controls.Expander> 控件由屏幕阅读器宣称为按钮。 从 .NET Framework 4.7.1 开始，它们被正确地称为可展开/可折叠组。

- 在 .NET Framework 4.7 及更低版本中，<xref:System.Windows.Controls.DataGridCell> 控件由屏幕阅读器宣称为“自定义”。 从 .NET Framework 4.7.1 开始，它们被正确地称为数据网格单元格（已本地化）。

- 从 .NET Framework 4.7.1 开始，屏幕阅读器宣布可编辑 <xref:System.Windows.Controls.ComboBox> 的名称。

- 在 .NET Framework 4.7 及更低版本中，<xref:System.Windows.Controls.PasswordBox> 控件被宣称为“视图中没有任何项”或有其他错误行为。 此问题已在 .NET Framework 4.7.1 及更高版本中解决。

**UIAutomation LiveRegion 支持**

屏幕阅读器（如讲述人）可帮助用户阅读应用程序的 UI 内容，通常通过具有焦点的 UI 内容的文本到语音转换输出实现。 但是，如果 UI 元素更改，并且不具有焦点，则用户可能不会收到通知，并且可能会错过重要信息。 活动区域旨在解决此问题。 开发人员可使用它们来通知屏幕阅读器或任何其他 UIAutomation 客户端 UI 元素有重要更改。 然后，屏幕阅读器可确定向用户通知此更改的方式和时间。

为了支持活动区域，向 WPF 添加了以下 API：

- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveSettingProperty?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveRegionChangedEvent?displayProperty=nameWithType> 字段，用于标识 LiveSetting 属性和 LiveRegionChanged 事件。 可通过使用 XAML 来进行设置。

- AutomationProperties.LiveSetting 附加属性，用于向屏幕阅读器通知 UI 更改对用户的重要性。

- <xref:System.Windows.Automation.AutomationProperties.LiveSettingProperty?displayProperty=nameWithType> 属性，用于标识 AutomationProperties.LiveSetting 附加属性。

- <xref:System.Windows.Automation.Peers.AutomationPeer.GetLiveSettingCore%2A?displayProperty=nameWithType> 方法，可替代该方法以提供 LiveSetting 值。

- <xref:System.Windows.Automation.AutomationProperties.GetLiveSetting%2A?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.AutomationProperties.SetLiveSetting%2A?displayProperty=nameWithType> 方法，用于获取和设置 LiveSetting 值。

- <xref:System.Windows.Automation.AutomationLiveSetting?displayProperty=nameWithType> 枚举，用于定义以下可能的 LiveSetting 值：

  - <xref:System.Windows.Automation.AutomationLiveSetting.Off?displayProperty=nameWithType>。 如果活动区域的内容已更改，则该元素不会发送通知。
  - <xref:System.Windows.Automation.AutomationLiveSetting.Polite?displayProperty=nameWithType>。 如果活动区域的内容已更改，则该元素将发送非中断通知。

  - <xref:System.Windows.Automation.AutomationLiveSetting.Assertive?displayProperty=nameWithType>. 如果活动区域的内容已更改，则该元素将发送中断通知。

可通过对相关元素设置 AutomationProperties.LiveSetting 属性来创建 LiveRegion，如以下示例所示：

```xaml
<TextBlock Name="myTextBlock" AutomationProperties.LiveSetting="Assertive">announcement</TextBlock>
```

活动区域中的数据发生更改，并且需要通知屏幕阅读器时，可显式引发事件，如以下示例所示。

```csharp
var peer = FrameworkElementAutomationPeer.FromElement(myTextBlock);

peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged);
```

```vb
Dim peer = FrameworkElementAutomationPeer.FromElement(myTextBlock)
peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged)

```

**高对比度**

从 .NET Framework 4.7.1 开始，对多种 WPF 控件进行了高对比度改进。 可在设置 <xref:System.Windows.SystemParameters.HighContrast%2A> 主题后看到这些改进。 这些方法包括：

- <xref:System.Windows.Controls.Expander> 控件

  <xref:System.Windows.Controls.Expander> 控件的焦点视觉对象现在可见。 <xref:System.Windows.Controls.ComboBox>、<xref:System.Windows.Controls.ListBox> 和 <xref:System.Windows.Controls.RadioButton> 控件的键盘视觉对象也可见。 例如：

  在此之前:

  ![此屏幕截图中显示了一个有焦点的组合框控件，其中没有焦点视觉对象。](./media/whats-new-in-accessibility/expander-control-before.png)

  之后：

  ![此屏幕截图中显示了一个有焦点的组合框控件，其中控件文本周围显示了虚线。](./media/whats-new-in-accessibility/expander-control-after.png)

- <xref:System.Windows.Controls.CheckBox> 和 <xref:System.Windows.Controls.RadioButton> 控件

  在高对比度主题下选中时，<xref:System.Windows.Controls.CheckBox> 和 <xref:System.Windows.Controls.RadioButton> 控件中的文本更易于查看。 例如：

  在此之前:

  ![此屏幕截图中显示了单选按钮和复选按钮，在高对比度主题下文本可见性差。](./media/whats-new-in-accessibility/high-contrast-radio-button-before.png)

  之后：

  ![高对比度主题上文本可见性高的单选和复选按钮的屏幕截图。](./media/whats-new-in-accessibility/high-contrast-radio-button-after.png)

- <xref:System.Windows.Controls.ComboBox> 控件

  从 .NET Framework 4.7.1 开始，已禁用的 <xref:System.Windows.Controls.ComboBox> 控件的边框与禁用的文本颜色相同。 例如：

  在此之前:

  ![此屏幕截图中显示了一个已禁用的组合框，其边框和空间文本具有不同的颜色。](./media/whats-new-in-accessibility/combo-disabled-before.png)

  之后：

  ![此屏幕截图中显示了一个已禁用的组合框，其边框和空间文本具有相同的颜色。](./media/whats-new-in-accessibility/combo-disabled-after.png)

  此外，已禁用的按钮和具有焦点的按钮使用正确的主题颜色。

  在此之前:

  ![此屏幕截图中显示了一个黑色按钮，按钮上有灰色文字“将焦点放到此处”。](./media/whats-new-in-accessibility/button-theme-colors-before.png)

  之后：

  ![带有黑色文本的蓝色按钮屏幕截图，文本显示“Focus Me”。](./media/whats-new-in-accessibility/button-theme-colors-after.png)

  最后，在 .NET Framework 4.7 及更低版本中，将 <xref:System.Windows.Controls.ComboBox> 控件的样式设置为 `Toolbar.ComboBoxStyleKey` 会导致下拉箭头不可见。 此问题已在 .NET Framework 4.7.1 及更高版本中解决。 例如：

  在此之前:

  ![此屏幕截图中显示了一个组合框控件，其中有一个不可见下拉箭头。](./media/whats-new-in-accessibility/combo-box-style-key-before.png)

  之后：

  ![显示下拉箭头的组合框控件的屏幕截图。](./media/whats-new-in-accessibility/combo-box-style-key-after.png)

- <xref:System.Windows.Controls.DataGrid> 控件

  从 .NET Framework 4.7.1 开始，<xref:System.Windows.Controls.DataGrid> 控件中的排序指示符箭头现在使用正确的主题颜色。 例如：

  在此之前:

  ![改进前的排序指示符箭头的屏幕截图。](./media/whats-new-in-accessibility/sort-indicator-before.png)

  之后：

  ![改进后的排序指示符箭头的屏幕截图。](./media/whats-new-in-accessibility/sort-indicator-after.png)

  此外，在 .NET Framework 4.7 及更低版本中，在高对比度模式下，默认链接样式在鼠标悬停在其上时更改为不正确的颜色。 此问题已在 .NET Framework 4.7.1 及更高版本中解决。 同样，从 .NET Framework 4.7.1 开始，<xref:System.Windows.Controls.DataGrid> 复选框列对键盘焦点反馈使用预期的颜色。

  在此之前:

  ![显示“点击此处！”的链接屏幕截图 文本是红色的。](./media/whats-new-in-accessibility/default-link-style-before.png)

  之后：

  ![显示“点击此处！”的链接屏幕截图 文本是黄色的。](./media/whats-new-in-accessibility/default-link-style-after.png)

有关 .NET Framework 4.7.1 中 WPF 辅助功能改进的详细信息，请参阅 [WPF 辅助功能改进](../migration-guide/retargeting/4.7-4.7.1.md#accessibility-improvements-in-wpf)。

<a name="winforms471"></a>

### <a name="windows-forms-accessibility-improvements"></a>Windows 窗体辅助功能改进

在 .NET Framework 4.7.1 中，Windows 窗体 (WinForms) 包括以下几个方面的辅助功能改进。

**改进了高对比度模式下的显示**

从 .NET Framework 4.7.1 开始，多种 WinForms 控件改进了高对比度模式下在操作系统中的呈现方式。 Windows 10 更改了一些高对比度系统颜色的值，而 Windows 窗体基于 Windows 10 Win32 框架。 为获得最佳体验，请运行最新版本的 Windows，并通过在测试应用程序中添加 app.manifest 文件选择使用最新的 OS 更改，同时取消注释 Windows 10 支持的 OS 行，最终结果如下所示：

```xml
<!-- Windows 10 -->
<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
```

高对比度更改的一些示例包括：

- <xref:System.Windows.Forms.MenuStrip> 项中的复选标记更易于查看。

- 选中后，禁用的 <xref:System.Windows.Forms.MenuStrip> 项更易于查看。

- 所选 <xref:System.Windows.Forms.Button> 控件中的文本与选中颜色形成鲜明对比。

- 禁用的文本更易于阅读。 例如：

  在此之前:

  ![一个应用的屏幕截图，该应用在高对比度模式下运行着不同的控件，而尚未改进可访问性。](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-before.png)

  之后：

  ![应用程序的屏幕截图，在辅助功能改进之后，该应用程序使用在高对比度模式下运行的不同控件。](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-after.png)

- “线程异常”对话框中的高对比度改进。

**改进了讲述人支持**

.NET Framework 4.7.1 中的 Windows 窗体对讲述人进行了以下辅助功改进：

- 讲述人以及其他 UI 自动化工具可访问 <xref:System.Windows.Forms.MonthCalendar> 控件。

- 当某个项的选中状态更改时，<xref:System.Windows.Forms.CheckedListBox> 控件会通知讲述人，因此用户可获知更改了列表项的值。

- <xref:System.Windows.Forms.DataGridViewCell> 控件向讲述人报告正确的只读状态。

- 讲述人现在可以阅读已禁用的 <xref:System.Windows.Forms.ToolStripMenuItem> 文本，而以前它会跳过禁用的菜单项。

**增强了对 UIAutomation 辅助功能模式的支持**

从 .NET Framework 4.7.1 开始，辅助功能技术工具的开发人员可以利用常见的 API 辅助功能模式和多个 WinForms 控件的属性。 这些辅助功能改进包括：

- <xref:System.Windows.Forms.ComboBox> 和 <xref:System.Windows.Forms.ToolStripSplitButton> 现在支持[展开/折叠模式](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)。

- <xref:System.Windows.Forms.DataGridViewCheckBoxCell> 现在支持[切换模式](../ui-automation/implementing-the-ui-automation-toggle-control-pattern.md)。

- <xref:System.Windows.Forms.ToolStripItem> 控件支持 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name> 属性和[展开/折叠模式](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)。

- <xref:System.Windows.Forms.NumericUpDown> 和 <xref:System.Windows.Forms.DomainUpDown> 控件支持 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name> 属性。

**改进了属性浏览器体验**

从 .NET Framework 4.7.1 开始，Windows 窗体包括以下改进：

- 更好地通过各种下拉选择窗口使用键盘导航。
- 减少不必要的制表位。
- 更好地报告控件类型。
- 改进了讲述人行为。

<a name="aspnet471"></a>

### <a name="aspnet-web-controls"></a>ASP.NET Web 控件

自 .NET Framework 4.7.1 和 Visual Studio 2017 版本 15.3 起，ASP.NET 改进了 ASP.NET Web 控件与 Visual Studio 中的辅助功能技术配合使用的方式。 包括以下更改：

- 在以下控件中实现缺失 UI 的辅助功能模式：例如“详细信息视图”向导中的“添加字段”对话框或“ListView”向导的“配置 ListView”对话框   。

- 改善在高对比度模式下（如“数据页导航字段编辑器”）的显示。

- 改善以下控件的键盘导航体验：例如 DataPager 控件的“编辑页导航字段”向导中的“字段”对话框、“配置 ObjectContext”对话框或“配置数据源”向导的“配置数据选择”对话框    。

<a name="tools471"></a>

### <a name="net-sdk-tools"></a>.NET SDK 工具

[配置编辑器工具 (SvcConfigEditor.exe)](../wcf/configuration-editor-tool-svcconfigeditor-exe.md) 和[服务跟踪查看器工具 (SvcTraceViewer.exe)](../wcf/service-trace-viewer-tool-svctraceviewer-exe.md) 通过修复各种辅助功能问题得到改进。 其中大多数都是一些小问题，如未定义名称或未正确实现某些 UI 自动化模式。 虽然许多用户不会意识到这些小问题的重要性，但使用屏幕阅读器等辅助技术的客户会发现这些 SDK 工具更易于访问。

这些改进更改了某些旧行为，例如键盘焦点顺序。

<a name="wf471"></a>

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Windows Workflow Foundation (WF) 工作流设计器

工作流设计器中的辅助功能更改包括：

- 某些控件中 Tab 键顺序更改为从左到右以及从上到下：

  - 设置 <xref:System.ServiceModel.Activities.InitializeCorrelation> 活动相关数据的初始化相关窗口。

  - <xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动的内容定义窗口。

- 通过键盘可以使用更多功能：

  - 编辑活动的属性时，属性组在第一次聚焦时可以通过键盘折叠。

  - 警告图标可以通过键盘访问。

  - “属性”窗口的“更多属性”按钮可以通过键盘访问 。

  - 键盘用户可以访问工作流设计器“参数”和“变量”窗格的标题项 。

- 提升了聚焦项的可见性，例如当：

  - 将行添加到工作流设计器和活动设计器使用的数据网格。

  - 在 <xref:System.ServiceModel.Activities.ReceiveReply> 和 <xref:System.ServiceModel.Activities.SendReply> 活动中按 Tab 键切换字段。

  - 设置变量或自变量的默认值

- 屏幕读取器现在可以正确识别：

  - 工作流设计器中设置的断点。

  - <xref:System.Activities.Statements.FlowSwitch%601>、<xref:System.Activities.Statements.FlowDecision> 和 <xref:System.ServiceModel.Activities.CorrelationScope> 活动。
  - <xref:System.ServiceModel.Activities.Receive> 活动的内容。

  - <xref:System.Activities.Statements.InvokeMethod> 活动的目标类型。

  - <xref:System.Activities.Statements.TryCatch> 活动中的“异常”组合框和“最终”部分。

  - 消息传递活动（<xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply> 和 <xref:System.ServiceModel.Activities.ReceiveReply>）中的“消息类型”组合框、“添加相关初始化表达式”窗口中的拆分器、“内容定义”窗口和“CorrelatesOn 定义”窗口。

  - 状态机转换和转换目标。

  - <xref:System.Activities.Statements.FlowDecision> 活动上的注释和连接器。

  - 活动的上下文（右键单击）菜单。

  - 属性值编辑器、“清除搜索”按钮、“按类别”和“按字母顺序”排序按钮以及属性网格中的“表达式编辑器”对话框。

  - 工作流设计器中的缩放百分比。

  - <xref:System.Activities.Statements.Parallel> 和 <xref:System.Activities.Statements.Pick> 活动中的分隔符。

  - <xref:System.Activities.Statements.InvokeDelegate> 活动。

  - 字典活动（`Microsoft.Activities.AddToDictionary<TKey,TValue>`、`Microsoft.Activities.RemoveFromDictionary<TKey,TValue>` 等）的“选择类型”窗口。

  - “浏览和选择 .NET 类型”窗口。

  - 工作流设计器中的痕迹导航。

- 选择高对比度主题的用户将看到工作流设计器及其控件可见性的许多改进，例如元素间更好的对比度和焦点元素更明显的选择框。

## <a name="see-also"></a>请参阅

- [.NET Framework 的新增功能](index.md)
