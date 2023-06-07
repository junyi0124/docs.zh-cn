---
title: 辅助功能最佳方案
description: 了解 .NET 中的辅助功能最佳实践。 探索编程访问、用户设置、可视化 UI 设计、导航和无模接口。
ms.date: 03/30/2017
helpviewer_keywords:
- best practices for accessibility
- accessibility, best practices for
ms.assetid: e6d5cd98-21a3-4b01-999c-fb953556d0e6
ms.openlocfilehash: 30961c83bdacf816856205322ac2b627d0f2594c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235683"
---
# <a name="accessibility-best-practices"></a>辅助功能最佳做法

> [!NOTE]
> 本文适用于想要使用命名空间中定义的托管 UI 自动化类的 .NET Framework 开发人员 <xref:System.Windows.Automation> 。 有关 UI 自动化的最新信息，请参阅 [Windows 自动化 API： Ui 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 在控件或应用程序中实现以下最佳做法将提高使用辅助技术设备的用户的可访问性。 其中许多最佳实践侧重于 (UI) 设计的良好用户界面。 每个最佳做法都包含 Windows Presentation Foundation (WPF) 控件或应用程序的实现信息。 在许多情况下，满足这些最佳做法的工作已包含在 WPF 控件中。  
  
<a name="Programmatic_Access"></a>

## <a name="programmatic-access"></a>以编程方式访问  

 编程访问涉及确保所有 UI 元素都已标记、公开了属性值并引发了相应的事件。 对于标准 WPF 控件，其中的大部分工作已通过完成 <xref:System.Windows.Automation.Peers.AutomationPeer> 。 自定义控件需要额外操作以确保正确实现编程访问。  
  
<a name="Enable_Programmatic_Access_to_all_UI_Elements_and_Text"></a>

### <a name="enable-programmatic-access-to-all-ui-elements-and-text"></a>对所有 UI 元素和文本启用编程访问  

 用户界面 (UI) 元素应启用编程访问。 如果 UI 是标准 WPF 控件，则控件中包含对编程访问的支持。 如果此控件是自定义控件（已是公共控件或“控件”的子类别），则必须检查可能需要修改的区域的 <xref:System.Windows.Automation.Peers.AutomationPeer> 实现。  
  
 遵循此最佳做法，辅助技术供应商可以标识和操作产品 UI 的元素。  
  
<a name="Place_Names__Titles_and_Descriptions_on_UI_Objects_"></a>

### <a name="place-names-titles-and-descriptions-on-ui-objects-frames-and-pages"></a>在 UI 对象、帧和页面上添加名称、标题和说明  

 辅助技术（尤其是屏幕阅读器）使用标题来了解导航方案中的帧、对象或页面的位置。 因此，标题必须是描述性的。 例如，如果用户已深入导航到某个特定区域，“Microsoft 网页”的网页标题毫无用处。 描述性标题对于有视力障碍且依赖屏幕阅读器的用户至关重要。 同样，对于 WPF 控件， <xref:System.Windows.Automation.AutomationProperties.NameProperty> 和对于 <xref:System.Windows.Automation.AutomationProperties.HelpTextProperty> 辅助技术设备十分重要。  
  
 遵循此最佳做法，辅助技术可以在示例控件和应用程序中识别和操作 UI。  
  
<a name="Ensure_Programmatic_Events_are_Triggered_by_all_UI"></a>

### <a name="ensure-programmatic-events-are-triggered-by-all-ui-activities"></a>确保编程事件可以由所有 UI 活动触发  

 遵循此最佳做法，辅助技术可以侦听 UI 中的更改并通知用户这些更改。  
  
<a name="User_Settings"></a>

## <a name="user-settings"></a>用户设置  

 本节中的最佳做法可确保控件或应用程序不重写用户设置。  
  
<a name="Respect_all_System_Wide_Settings_and_do_not_Interfere"></a>

### <a name="respect-all-system-wide-settings-and-do-not-interfere-with-accessibility-functions"></a>请遵守所有系统范围内设置，同时请勿干扰辅助功能函数  

 用户可以使用“控制面板”设置一些系统范围内标志；而其他标志可通过编程方式进行设置。 控件或应用程序不应更改这些设置。 此外，应用程序必须支持其主机操作系统的辅助功能设置。  
  
 遵循此最佳做法，用户可设置辅助功能设置并了解应用程序将不会更改这些设置。  
  
<a name="Visual_UI_Design"></a>

## <a name="visual-ui-design"></a>可视 UI 设计  

 本节中的最佳做法可确保控件或应用程序有效地使用颜色和图像，并可供辅助技术使用。  
  
<a name="Don_t_Hard_Code_Colors"></a>

### <a name="dont-hard-code-colors"></a>请勿进行硬编码颜色  

 色盲、视力较差，或者使用黑白屏幕的用户可能无法使用带有硬编码颜色的应用程序。  
  
 遵循此最佳做法，用户可根据各自需求调整颜色组合。  
  
<a name="Support_High_Contrast_and_all_System_Display_Attributes"></a>

### <a name="support-high-contrast-and-all-system-display-attributes"></a>支持高对比度和所有系统显示特性  

 应用程序不应中断或禁用用户选定的系统范围内对比度设置、颜色选择或其他系统范围内显示设置和特性。 用户采用的系统范围内设置可增强应用程序的辅助功能，因此应用程序不应禁用或忽略它。 应在前景色背景色组合中正确运用颜色，以确保提供适当的对比度。 不要混合使用不相关的颜色，也不要反转颜色。  
  
 很多用户需要特定的高对比度组合，例如黑色背景中的白色文本。 绘制这些对比色（如白色背景上的黑色文本）会导致背景渗入前景，使一些用户阅读困难。  
  
<a name="Ensure_all_UI_Correctly_Scales_by_any_DPI_Setting"></a>

### <a name="ensure-all-ui-correctly-scales-by-any-dpi-setting"></a>确保任何 DPI 设置可以正确缩放所有 UI  

 确保所有 UI 可以按英寸的任何点进行正确缩放 (dpi) 设置。 此外，请确保 UI 元素适合 1024 x 768 屏幕，每英寸120点 (dpi) 。  
  
<a name="Navigation"></a>

## <a name="navigation"></a>导航  

 本节中的最佳做法可确保解决控件和应用程序的导航问题。  
  
<a name="Provide_Keyboard_Interface_for_all_UI_Elements"></a>

### <a name="provide-keyboard-interface-for-all-ui-elements"></a>为所有 UI 元素提供键盘界面  

 制表位停止，特别是在仔细规划时，为用户授予另一种导航 UI 的方法。  
  
 应用程序应提供以下键盘界面：  
  
- 用户可与之交互的所有控件的制表位，例如按钮、链接或列表框  
  
- 逻辑 Tab 键顺序  
  
<a name="Show_the_Keyboard_Focus"></a>

### <a name="show-the-keyboard-focus"></a>显示键盘焦点  

 用户需要知道哪个对象具有键盘焦点，以便可以预测击键效果。 若要突出显示键盘焦点，请使用颜色、字体或图形（如矩形）或放大倍数。 若要呼叫时突出显示键盘焦点，请更改音量、音调或音质。  
  
 为避免混淆，应用程序应隐藏所有可视焦点指示器并调暗位于非活动窗口（或窗格）中的选择内容。  
  
 应用程序使用键盘焦点执行以下操作：  
  
- 始终应有一个项具有键盘焦点  
  
- 键盘焦点应该为可见和明显  
  
- 选择和/或带有焦点的项应以可视化方式突出显示  
  
<a name="Support_Navigation_Standards_and_Powerful_Navigation"></a>

### <a name="support-navigation-standards-and-powerful-navigation-schemes"></a>支持导航标准和功能强大的导航方案  

 键盘导航的不同方面为用户提供了用于浏览 UI 的不同方式。  
  
 应用程序应提供以下键盘界面：  
  
- 所有命令、菜单和控件的快捷键和带下划线的访问键  
  
- 重要链接的键盘快捷键  
  
- 所有菜单项都具有一个访问键；所有按钮都有加速键，所有命令都具有一个加速键。  
  
<a name="Do_not_let_Mouse_Location_Interfere_with_Keyboard"></a>

### <a name="do-not-let-mouse-location-interfere-with-keyboard-navigation"></a>不要让鼠标位置干扰键盘导航  

 鼠标位置不应干扰键盘导航。 例如，如果鼠标定位在某个位置并且用户使用键盘导航，则不应发生鼠标单击，除非用户启动。  
  
<a name="Multimodal_Interface"></a>

## <a name="multimodal-interface"></a>多模式界面  

 本节中的最佳做法可确保应用程序 UI 包含视觉对象元素的替代项。  
  
<a name="Provide_User_Selectable_Equivalents_for_Non_Text"></a>

### <a name="provide-user-selectable-equivalents-for-non-text-elements"></a>提供非文本元素的用户可选等效项  

 对于每个非文本元素，都会提供文本、脚本或音频说明（如 alt 文本、标题或可视反馈）的用户可选等效项。  
  
 非文本元素涵盖各种 UI 元素，包括图像、图像映射区域、动画、小程序、框架、脚本、图形按钮、声音、独立的音频文件和视频。 当非文本元素包含用户需要访问的可视信息、语音或一般音频信息时，这些元素很重要，以便了解 UI 的内容。  
  
<a name="Use_Color_but_also_Provide_Alternatives_to_Color"></a>

### <a name="use-color-but-also-provide-alternatives-to-color"></a>使用颜色，且同时提供颜色的替换项  

 使用颜色来增强、强调或重申通过其他方式显示的信息，但请勿仅使用颜色传达信息。 色盲或者使用单色显示的用户需要颜色的替换项。  
  
<a name="Use_Standard_Input_APIs_with_Devices_Independent"></a>

### <a name="use-standard-input-apis-with-device-independent-calls"></a>通过独立于设备的调用使用标准输入 API  

 独立于设备的调用确保键盘和鼠标的功能相等，同时为辅助技术提供有关 UI 的所需信息。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.Peers>
- [带有主题和 UI 自动化支持示例的 NumericUpDown 自定义控件](/previous-versions/dotnet/netframework-3.5/ms771573(v=vs.90))
- [键盘用户界面设计的准则](/previous-versions/windows/desktop/dnacc/guidelines-for-keyboard-user-interface-design)
