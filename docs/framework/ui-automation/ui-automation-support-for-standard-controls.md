---
title: UI 自动化对标准控件的支持
description: 获取有关针对 Windows Presentation Foundation (WPF) 、Win32 和 Windows 窗体开发的应用程序中的标准控件的 UI 自动化支持的信息。
ms.date: 03/30/2017
helpviewer_keywords:
- controls, UI Automation support for
- UI Automation, support for standard controls
ms.assetid: 3770ea8a-2655-4add-9c59-fe0610ad5084
ms.openlocfilehash: 0a5a0b61a6492d9efb62799fa610859b247cf26e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261067"
---
# <a name="ui-automation-support-for-standard-controls"></a>UI 自动化对标准控件的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题包含有关为 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 、Win32 和 Windows 窗体框架开发的应用程序中标准控件的支持的信息。  
  
<a name="Windows_Presentation_Foundation_Controls"></a>

## <a name="windows-presentation-foundation-controls"></a>Windows Presentation Foundation 控件  

 为用户交互提供信息或支持的所有 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 控件元素均具有对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的完整本机支持。 面板等其他元素对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]不可见。  
  
<a name="Win32_Controls"></a>

## <a name="win32-controls"></a>Win32 控件  

 大多数 Win32 控件都 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 通过 UIAutomationClientsideProviders.dll 中的客户端提供程序公开。 此程序集将自动注册，以用于 UI 自动化客户端应用程序。  
  
 仅为 *ComCtrl32.dll* 版本6中的控件提供完全支持。  
  
 支持以下控件。  
  
|类名|控件类型|  
|----------------|------------------|  
|Button|Button|  
|Button|RadioButton|  
|Button|组|  
|Button|CheckBox|  
|Button|Hyperlink|  
|Button|SplitButton|  
|Button|CheckBox|  
|ComboBoxEx32|组合框|  
|组合框|组合框|  
|编辑|文档|  
|编辑|编辑|  
|SysLink|Hyperlink|  
|Static|文本|  
|Static|映像|  
|SysIPAddress32|自定义|  
|SysHeader32|Header/HeaderItem|  
|SysListView32|DataGrid|  
|SysListView32|列出|  
|ListBox|列出|  
|ListBox|ListItem|  
|#32768|菜单|  
|#32768|MenuItem|  
|msctls_progress32|ProgressBar|  
|RichEdit|Document。 查看注释。|  
|RichEdit20A|文档|  
|RichEdit20W|文档|  
|RichEdit50W|文档|  
|ScrollBar|Slider|  
|msctls_trackbar32|Slider|  
|msctls_updown32|Spinner|  
|msctls_statusbar32|StatusBar|  
|SysTabControl32|选项卡|  
|SysTabControl32|TabItem|  
|ToolbarWindow32|ToolBar|  
|ToolbarWindow32|MenuItem|  
|ToolbarWindow32|Button|  
|ToolbarWindow32|CheckBox|  
|ToolbarWindow32|RadioButton|  
|ToolbarWindow32|Separator|  
|tooltips_class32|ToolTip|  
|#32774|ToolTip|  
|ReBarWindow32|Toolbar|  
|SysTreeView32|树|  
|SysTreeView32|TreeItem|  
  
 **注意** 只有 Windows Vista (附带 RichEd20.dll 版本3.1 及更高版本中提供的版本和 MsftEdit.dll 版本4.1 及更高版本) 支持 RichEdit 控件。  
  
 不支持以下控件。  
  
|类名|控件类型|  
|----------------|------------------|  
|SysAnimate32|映像|  
|SysPager|Spinner|  
|SysDateTimePick32|自定义|  
|SysMonthCal32|日历|  
|MS_WINNOTE|工具提示|  
|VBBubble|工具提示|  
|ScrollBar（在用作独立控件时）|Slider|  
|SuperGrid|自定义|  
  
<a name="Windows_Forms_Controls"></a>

## <a name="windows-forms-controls"></a>Windows 窗体控件  

 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]通过 UIAutomationClientsideProviders.dll 中的客户端提供程序向 Windows 窗体控件公开。 此程序集将自动注册，以用于 UI 自动化客户端应用程序。  
  
 通常，支持作为 Win32 公共控件的托管包装器 Windows 窗体控件 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 。 支持以下控件。  
  
|类名|  
|----------------|  
|Button|  
|CheckBox|  
|CheckedListBox|  
|ColorDialog|  
|组合框|  
|FolderBrowser|  
|FontDialog|  
|GroupBox|  
|HscrollBar|  
|ImageList|  
|Label|  
|ListBox|  
|ListView|  
|MainMenu/ContextMenu|  
|MonthCalendar|  
|NotifyIcon|  
|OpenFileDialog|  
|PageSetupDialog|  
|PrintDialog|  
|ProgressBar|  
|RadioButton|  
|RichTextBox|  
|SaveFileDialog|  
|ScrollableControl|  
|SoundPlayer|  
|StatusBar|  
|TabControl/TabPage|  
|TextBox|  
|计时器|  
|Toolbar|  
|ToolTip|  
|TrackBar|  
|TreeView|  
|VscrollBar|  
|WebBrowser|  
  
 以下控件 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 仅通过其对 Microsoft Active Accessibility 的支持公开。 某些功能可能不可用。  
  
|控件名称|  
|------------------|  
|BindingSource|  
|DataGrid|  
|DataGridView|  
|DataNavigator|  
|DomainUpDown|  
|ErrorProvider|  
|FlowLayoutPanel|  
|窗体|  
|LinkLabel|  
|HelpProvider|  
|MaskedTextBox|  
|MenuStrip/ContextMenuStrip|  
|NumericUpDown|  
|Panel|  
|PictureBox|  
|PrintDocument|  
|PrintPreview-Control|  
|PrintPreview-Dialog|  
|PropertyGrid|  
|用户控件|  
|ToolStrip|  
|TableLayoutPanel|  
|SplitContainer/SplitterPanel|  
|Splitter|  
|RaftingContainer|  
|StatusStrip|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件类型](ui-automation-control-types.md)
