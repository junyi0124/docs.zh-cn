---
ms.openlocfilehash: 895d09e4ec39bd4a92ed1f4910da80474334d99b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497253"
---
### <a name="accessibility-improvements-in-wpf"></a>WPF 中辅助功能的改进

#### <a name="details"></a>详细信息

**高对比度改进**

- <xref:System.Windows.Controls.Expander> 控件的焦点现在可见。 在先前版本的 .NET Framework 中，并非如此。
- <xref:System.Windows.Controls.CheckBox> 和 <xref:System.Windows.Controls.RadioButton> 控件中的文本在选中时，比之前的 .NET Framework 版本更易查看。
- 现在已禁用的 <xref:System.Windows.Controls.ComboBox> 的边框颜色与已禁用的文本颜色相同。 在先前版本的 .NET Framework 中，并非如此。
- 禁用和聚焦的按钮现在使用正确的主题颜色。 在先前版本的 .NET Framework 中，并未如此。
- 当 <xref:System.Windows.Controls.ComboBox> 控件的样式设置为 <xref:System.Windows.Controls.ToolBar.ComboBoxStyleKey?displayProperty=nameWithType> 时，下拉按钮现在可见。 在先前版本的 .NET Framework 中，并非如此。
- <xref:System.Windows.Controls.DataGrid> 控件中的排序指示器箭头现在可使用主题颜色。 在先前版本的 .NET Framework 中，并非如此。
- 默认超链接样式更改为在鼠标悬停时显示正确的主题颜色。 在先前版本的 .NET Framework 中，并非如此。
- 单选按钮上的键盘焦点现在可见。 在先前版本的 .NET Framework 中，并非如此。
- 现在 <xref:System.Windows.Controls.DataGrid> 控件的复选框列对键盘焦点反馈使用预期的颜色。 在先前版本的 .NET Framework 中，并非如此。
- 现在，键盘焦点视觉对象在 <xref:System.Windows.Controls.ComboBox> 和 <xref:System.Windows.Controls.ListBox> 控件上可见。 在先前版本的 .NET Framework 中，并非如此。

**屏幕阅读器交互改进**

- 屏幕阅读器现在正确地将 <xref:System.Windows.Controls.Expander> 控件称为组（展开/折叠）。
- 屏幕阅读器现在正确地将 <xref:System.Windows.Controls.DataGridCell> 控件称为数据网格单元格（已本地化）。
- 屏幕阅读器现在将宣布可编辑的 <xref:System.Windows.Controls.ComboBox> 的名称。
- 屏幕阅读器不再将 <xref:System.Windows.Controls.PasswordBox> 控件读作“视图中没有任何项”。

**LiveRegion 支持**

屏幕阅读器（例如讲述人）可帮助用户理解应用程序的用户界面 (UI)，这通常是通过描述当前具有焦点的 UI 元素来实现的。 但是，如果 UI 元素更改了屏幕中某些地方，并且不具有焦点，则用户可能不会收到通知，并且错过重要信息。 LiveRegions 旨在解决此问题。 开发人员可使用它们来通知屏幕阅读器或任何其他 [UI 自动化](~/docs/framework/ui-automation/ui-automation-overview.md)客户端 UI 元素有重要更改。 然后，屏幕阅读器可确定向用户通知此更改的方式和时间。 LiveSetting 属性还让屏幕阅读器知道了向用户通知 UI 更改的重要性。

#### <a name="suggestion"></a>建议

**如何选择启用或弃用这些更改**

为了使应用程序从这些更改中获益，它必须在 .NET Framework 4.7.1 或更高版本上运行。 此应用程序可通过以下任何一种方式从这些更改中获益：

- 面向 .NET Framework 4.7.1。 此为推荐方法。 对于面向 .NET Framework 4.7.1 或更高版本的 WPF 应用程序，这些辅助功能默认启用。
- 通过向应用配置文件的 `<runtime>` 部分添加以下 [AppContext 开关](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)并将其设置为 `false`，可选择弃用旧版辅助功能行为，如下例所示。

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7"/>
    </startup>
    <runtime>
      <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true/false;key2=true/false'  -->
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
    </runtime>
  </configuration>
  ```

如果应用程序面向 .NET Framework 4.7.1 或更高版本并希望保留旧版辅助功能行为，则它们可通过将此 AppContext 开关显式设置为 `true` 来选择启用旧版辅助功能。
有关 UI 自动化的概述，请参阅 [UI 自动化概述](~/docs/framework/ui-automation/ui-automation-overview.md)。

| 名称    | 值       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveSettingProperty?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveRegionChangedEvent?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationLiveSetting?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.LiveSettingProperty?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.SetLiveSetting(System.Windows.DependencyObject,System.Windows.Automation.AutomationLiveSetting)?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.GetLiveSetting(System.Windows.DependencyObject)?displayProperty=nameWithType>
- <xref:System.Windows.Automation.Peers.AutomationPeer.GetLiveSettingCore?displayProperty=nameWithType>
