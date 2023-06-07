---
title: UI 自动化和 Microsoft Active Accessibility
description: 了解 UI 自动化和 Microsoft Active Accessibility 之间的差异，这是使应用程序可访问的早期解决方案。
ms.date: 03/30/2017
helpviewer_keywords:
- Active Accessibility
- UI Automation, Active Accessibility compared to
- UI Automation, Microsoft Active Accessibility
- Active Accessibility, UI Automation compared to
ms.assetid: 87bee662-0a3e-4232-a421-20e7a5968321
ms.openlocfilehash: 79545c292cf0f04620a78105833a2f2c76dc5f78
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242001"
---
# <a name="ui-automation-and-microsoft-active-accessibility"></a>UI 自动化和 Microsoft Active Accessibility

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 Microsoft Active Accessibility 是使应用程序可访问的早期解决方案。 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 是 Microsoft Windows 的新辅助功能模型，旨在满足辅助技术产品和自动测试工具的需求。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 通过 Active Accessibility 提供了许多改进。  
  
 本主题包括的主要功能 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，并解释了这些功能与 Active Accessibility 的不同之处。  
  
<a name="Programming_Languages_compare"></a>

## <a name="programming-languages"></a>编程语言  

Active Accessibility 基于组件对象模型 (COM) 支持双重接口，因此可在 C/c + +、Microsoft Visual Basic 6.0 和脚本语言中进行编程。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] (包括用于标准控件的客户端提供程序库) 使用托管代码编写，并且 UI 自动化客户端应用程序可以使用 c # 或 Visual Basic .NET 进行编程。 作为接口实现的 UI 自动化提供程序可以用托管代码或 C/C++ 编写。  
  
<a name="Support_in_Windows_Presentation_Foundation_"></a>

## <a name="support-in-windows-presentation-foundation"></a>Windows Presentation Foundation 中的支持  

 Windows Presentation Foundation (WPF) 是用于创建用户界面的新模型。 WPF 元素不包含对 Active Accessibility 的本机支持;但是，它们支持 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，其中包括对 Active Accessibility 客户端的桥接支持。 只有专为编写的客户端 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 才能充分利用 WPF 的辅助功能，例如对文本的丰富支持。  
  
<a name="Servers_and_Clients_compare"></a>

## <a name="servers-and-clients"></a>服务器和客户端  

 在 Active Accessibility 中，服务器和客户端通过服务器的实现直接进行通信 `IAccessible` 。  
  
 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，核心服务位于服务器（称为提供程序）和客户端之间。 核心服务对由提供程序实现的接口进行调用并提供其他服务，例如，为元素生成唯一的运行时标识符。 客户端应用程序使用库函数来调用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 服务。  
  
 UI 自动化提供程序可以向 Active Accessibility 客户端提供信息，Active Accessibility 服务器可以向 UI 自动化客户端应用程序提供信息。 但是，因为 Active Accessibility 不会公开尽可能多的信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，所以这两个模型并不完全兼容。  
  
<a name="UI_Elements_compare"></a>

## <a name="ui-elements"></a>UI 元素  

 Active Accessibility [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 以 `IAccessible` 接口或子标识符的形式呈现元素。 很难比较两个 `IAccessible` 指针来确定它们是否引用同一元素。  
  
 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，每个元素都表示为一个 <xref:System.Windows.Automation.AutomationElement> 对象。 可通过使用相等运算符或 <xref:System.Windows.Automation.AutomationElement.Equals%2A> 方法完成比较，这两种方法比较的都是元素的唯一运行时标识符。  
  
<a name="Tree_Views_and_Navigation_compare"></a>

## <a name="tree-views-and-navigation"></a>树视图和导航  

 屏幕上的 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 元素可以被视为树状结构，桌面作为根、应用程序窗口作为直接子项，而应用程序中的元素作为继承子项。  
  
 在 Active Accessibility 中，在树中公开了许多与最终用户不相关的自动化元素。 客户端应用程序需要查看所有元素以确定哪些元素是有意义的。  
  
 UI 自动化客户端应用程序通过筛选视图查看 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 。 该视图仅包含相关元素：向用户提供信息或实现交互的元素。 还提供了只包含控件元素和只包含内容元素的预定义视图；除此之外，应用程序还可以定义自定义视图。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 简化了向用户描述 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 和帮助用户与应用程序进行交互的任务。  
  
 在 Active Accessibility 中，在元素之间进行导航的空间 (例如，移到位于屏幕左侧的元素) 、逻辑 (例如，移到下一个菜单项，或对话框中的 tab 键顺序中的下一项) 或分层 (例如，将容器中的第一个子元素或从子级移动到其父级的) 。 分层导航非常复杂，因为子元素并不总是实现 `IAccessible`的对象。  
  
 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 元素都是支持相同基本功能的 <xref:System.Windows.Automation.AutomationElement> 对象。  (从提供程序的角度来看，它们是实现从继承的接口的对象 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple> 。 ) 导航主要是层次结构：从父级到子级，以及从一个同级到下一个同级。 同级之间 (导航具有一个逻辑元素，因为它可能遵循 tab 键顺序。 ) 可以通过使用类，使用树的任何筛选视图从任何起始点导航。 <xref:System.Windows.Automation.TreeWalker> 你还可以通过使用 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> 和 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>导航到特定的子级或继承项；例如，很容易检索对话框中支持指定控件模式的所有元素。  
  
 中的导航 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 比在 Active Accessibility 中的导航更加一致。 某些元素（例如下拉列表和弹出窗口）在 Active Accessibility 树中出现两次，从它们导航可能会产生意外的结果。 实际上不可能为 rebar 控件正确实现 Active Accessibility。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 能够重排根目录并进行重新布置，因此可以在树的任何位置放置元素，不必考虑窗口所有者构建的层次结构。  
  
<a name="Roles_and_Control_Types"></a>

## <a name="roles-and-control-types"></a>角色和控件类型  

 Active Accessibility 使用 `accRole` () 的属性 `IAccessible::get_actRole` 来检索中元素的角色说明 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] ，如 ROLE_SYSTEM_SLIDER 或 ROLE_SYSTEM_MENUITEM。 元素的角色是确定其可用功能的主要线索。 通过使用固定的方法（如 `IAccessible::accSelect` 和 `IAccessible::accDoDefaultAction`）可实现与控件的交互。 客户端应用程序和 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 之间的交互仅限于可通过 `IAccessible`完成的交互。  
  
 与此相反， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 很大程度上会将该元素的控件类型（由 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> 属性描述）从其预期的功能分离。 功能由通过实现其特殊化接口的提供程序支持的控件模式来确定。 可以结合控件模式来描述特定 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 元素支持的完整功能集。 某些提供程序需要支持特定的控件模式；例如，复选框的提供程序必须支持 Toggle 控件模式。 其他提供程序需要支持一个或多个控件模式集；例如，一个按钮必须支持 Toggle 或 Invoke。 还有一些提供程序完全不支持任何控件模式；例如，不能移动、调整大小或停靠的窗格没有任何控件模式。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持由 <xref:System.Windows.Automation.ControlType.Custom> 属性标识的自定义控件且可通过 <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> 属性描述。  
  
 下表显示 Active Accessibility 角色到控件类型的映射 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 。  
  
|Active Accessibility 角色|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件类型|  
|----------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
|ROLE_SYSTEM_PUSHBUTTON|Button|  
|ROLE_SYSTEM_CLIENT|日历|  
|ROLE_SYSTEM_CHECKBUTTON|复选框|  
|ROLE_SYSTEM_COMBOBOX|组合框|  
|ROLE_SYSTEM_CLIENT|自定义|  
|ROLE_SYSTEM_LIST|数据网格|  
|ROLE_SYSTEM_LISTITEM|数据项|  
|ROLE_SYSTEM_DOCUMENT|文档|  
|ROLE_SYSTEM_TEXT|编辑|  
|ROLE_SYSTEM_GROUPING|组|  
|ROLE_SYSTEM_LIST|标头|  
|ROLE_SYSTEM_COLUMNHEADER|标头项|  
|ROLE_SYSTEM_LINK|Hyperlink|  
|ROLE_SYSTEM_GRAPHIC|映像|  
|ROLE_SYSTEM_LIST|列出|  
|ROLE_SYSTEM_LISTITEM|列表项|  
|ROLE_SYSTEM_MENUPOPUP|菜单|  
|ROLE_SYSTEM_MENUBAR|菜单栏|  
|ROLE_SYSTEM_MENUITEM|Menu item|  
|ROLE_SYSTEM_PANE|窗格|  
|ROLE_SYSTEM_PROGRESSBAR|进度条|  
|ROLE_SYSTEM_RADIOBUTTON|单选按钮|  
|ROLE_SYSTEM_SCROLLBAR|滚动条|  
|ROLE_SYSTEM_SEPARATOR|Separator|  
|ROLE_SYSTEM_SLIDER|Slider|  
|ROLE_SYSTEM_SPINBUTTON|Spinner|  
|ROLE_SYSTEM_SPLITBUTTON|“拆分”按钮|  
|ROLE_SYSTEM_STATUSBAR|状态栏|  
|ROLE_SYSTEM_PAGETABLIST|选项卡|  
|ROLE_SYSTEM_PAGETAB|选项卡项|  
|ROLE_SYSTEM_TABLE|表|  
|ROLE_SYSTEM_STATICTEXT|文本|  
|ROLE_SYSTEM_INDICATOR|Thumb|  
|ROLE_SYSTEM_TITLEBAR|标题栏|  
|ROLE_SYSTEM_TOOLBAR|工具栏|  
|ROLE_SYSTEM_TOOLTIP|ToolTip|  
|ROLE_SYSTEM_OUTLINE|树|  
|ROLE_SYSTEM_OUTLINEITEM|树项|  
|ROLE_SYSTEM_WINDOW|窗口|  
  
 有关不同控件类型的更多信息，请参阅 [UI Automation Control Types](ui-automation-control-types.md)。  
  
<a name="States_and_Properties"></a>

## <a name="states-and-properties"></a>状态和属性  

 在 Active Accessibility 中，元素支持一组通用的属性，某些属性 (如 `accState`) 必须描述非常不同的内容，具体取决于元素的角色。 服务器必须实现可返回属性的 `IAccessible` 的所有方法，返回的属性甚至与该元素不相关。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 定义更多的属性，其中一些属性与 Active Accessibility 中的状态相对应。 某些属性是所有元素共有的，但其他属性特定于控件类型和控件模式。 属性由唯一标识符进行区分，并且大多数属性都可通过使用单个方法 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> 或 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>进行检索。 许多属性还可以轻松从 <xref:System.Windows.Automation.AutomationElement.Current%2A> 和 <xref:System.Windows.Automation.AutomationElement.Cached%2A> 属性访问器进行检索。  
  
 UI 自动化提供程序不需要实现不相关的属性，而可以只返回任何其不支持的属性的 `null` 值。 此外， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 核心服务可以从默认窗口提供程序获取一些属性，并且这些属性与提供程序显式实现的属性相合并。  
  
 除了支持更多属性外， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 通过允许使用单一跨进程调用检索多个属性来提供更好的性能。  
  
 下表显示了两个模型中属性之间的对应关系。  
  
|Active Accessibility 属性访问器|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性 ID|备注|  
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------|  
|`get_accKeyboardShortcut`|<xref:System.Windows.Automation.AutomationElement.AccessKeyProperty> 或 <xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>|如果两者都存在，`AccessKeyProperty` 优先。|  
|`get_accName`|<xref:System.Windows.Automation.AutomationElement.NameProperty>||  
|`get_accRole`|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty>|请参阅上表，了解角色到控件类型的映射。|  
|`get_accValue`|<xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType>|仅对支持 ValuePattern 或 RangeValuePattern 的控件类型有效。 RangeValue 值被规范化为 0-100，与 MSAA 行为保持一致。 值项使用一个字符串。|  
|`get_accHelp`|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>||  
|`accLocation`|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>||  
|`get_accDescription`|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不受支持|`accDescription` 在 MSAA 内没有清晰的规范，这将导致提供程序在此属性中放置不同的信息片段。|  
|`get_accHelpTopic`|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不受支持||  
  
 下表显示了哪些 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性对应于 Active Accessibility 状态常量。  
  
|Active Accessibility 状态|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|是否触发状态更改？|  
|-----------------------------------------------------------------------|------------------------------------------------------------------------------------|----------------------------|  
|STATE_SYSTEM_CHECKED|对于复选框， <xref:System.Windows.Automation.TogglePattern.ToggleStateProperty><br /><br /> 对于单选按钮， <xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|Y|  
|STATE_SYSTEM_COLLAPSED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Collapsed>|Y|  
|STATE_SYSTEM_EXPANDED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Expanded> 或 <xref:System.Windows.Automation.ExpandCollapseState.PartiallyExpanded>|Y|  
|STATE_SYSTEM_FOCUSABLE|<xref:System.Windows.Automation.AutomationElement.IsKeyboardFocusableProperty>|N|  
|STATE_SYSTEM_FOCUSED|<xref:System.Windows.Automation.AutomationElement.HasKeyboardFocusProperty>|N|  
|STATE_SYSTEM_HASPOPUP|针对菜单项的<xref:System.Windows.Automation.ExpandCollapsePattern>|N|  
|STATE_SYSTEM_INVISIBLE|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> = True 且 <xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A> 导致 <xref:System.Windows.Automation.NoClickablePointException>|N|  
|STATE_SYSTEM_LINKED|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> =<br /><br /> <xref:System.Windows.Automation.ControlType.Hyperlink>|N|  
|STATE_SYSTEM_MIXED|<xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> = <xref:System.Windows.Automation.ToggleState.Indeterminate>|N|  
|STATE_SYSTEM_MOVEABLE|<xref:System.Windows.Automation.TransformPattern.CanMoveProperty>|N|  
|STATE_SYSTEM_MUTLISELECTABLE|<xref:System.Windows.Automation.SelectionPattern.CanSelectMultipleProperty>|N|  
|STATE_SYSTEM_OFFSCREEN|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> = True|N|  
|STATE_SYSTEM_PROTECTED|<xref:System.Windows.Automation.AutomationElement.IsPasswordProperty>|N|  
|STATE_SYSTEM_READONLY|<xref:System.Windows.Automation.RangeValuePattern.IsReadOnlyProperty?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty?displayProperty=nameWithType>|N|  
|STATE_SYSTEM_SELECTABLE|支持 <xref:System.Windows.Automation.SelectionItemPattern>|N|  
|STATE_SYSTEM_SELECTED|<xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|N|  
|STATE_SYSTEM_SIZEABLE|<xref:System.Windows.Automation.TransformPattern.TransformPatternInformation.CanResize%2A>|N|  
|STATE_SYSTEM_UNAVAILABLE|<xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>|Y|  
  
 以下状态未由大多数 Active Accessibility 控制服务器实现，或者在中没有等效项 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 。  
  
|Active Accessibility 状态|备注|  
|-----------------------------------------------------------------------|-------------|  
|STATE_SYSTEM_BUSY|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
|STATE_SYSTEM_DEFAULT|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
|STATE_SYSTEM_ANIMATED|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
|STATE_SYSTEM_EXTSELECTABLE|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_MARQUEED|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_SELFVOICING|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_TRAVERSED|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
|STATE_SYSTEM_ALERT_HIGH|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_ALERT_MEDIUM|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_ALERT_LOW|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_FLOATING|不是由 Active Accessibility 服务器广泛实现|  
|STATE_SYSTEM_HOTTRACKED|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
|STATE_SYSTEM_PRESSED|在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中不可用|  
  
 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性标识符的完整列表，请参阅 [UI Automation Properties Overview](ui-automation-properties-overview.md)。  
  
<a name="uiautomation_events_compare"></a>

## <a name="events"></a>事件  

 与 Active Accessibility 中的事件机制不同，中的事件机制 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 不依赖于 Windows 事件路由 (，它在窗口句柄) 密切相关，并且不需要客户端应用程序来设置挂钩。 可以对事件订阅进行微调，不仅能订阅特定事件，甚至还能订阅树的特定部分。 通过跟踪所侦听的事件，提供程序也可微调其事件的引发。  
  
 对于客户端来说检索引发事件的元素也很简单，因为这些事件会被直接传递到事件回调。 如果在客户端订阅该事件时，缓存请求处于活动状态，则会自动预提取元素的属性。  
  
 下表显示 Active Accessibility 了 winevent 和事件的对应关系 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 。  
  
|WinEvent|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件标识符|  
|--------------|--------------------------------------------------------------------------------------------|  
|EVENT_OBJECT_ACCELERATORCHANGE|<xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty> 属性更改|  
|EVENT_OBJECT_CONTENTSCROLLED|相关联的滚动条上的 或  属性更改|  
|EVENT_OBJECT_CREATE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_DEFACTIONCHANGE|无等效项|  
|EVENT_OBJECT_DESCRIPTIONCHANGE|没有确切的等效项；也许 <xref:System.Windows.Automation.AutomationElement.HelpTextProperty> 或 <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> 属性更改|  
|EVENT_OBJECT_DESTROY|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_FOCUS|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_OBJECT_HELPCHANGE|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty> 已更改|  
|EVENT_OBJECT_HIDE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_LOCATIONCHANGE|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> 属性更改|  
|EVENT_OBJECT_NAMECHANGE|<xref:System.Windows.Automation.AutomationElement.NameProperty> 属性更改|  
|EVENT_OBJECT_PARENTCHANGE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_REORDER|不一致地用于 Active Accessibility。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中没有定义直接对应的事件。|  
|EVENT_OBJECT_SELECTION|<xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent>|  
|EVENT_OBJECT_SELECTIONADD|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent>|  
|EVENT_OBJECT_SELECTIONREMOVE|<xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent>|  
|EVENT_OBJECT_SELECTIONWITHIN|无等效项|  
|EVENT_OBJECT_SHOW|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_STATECHANGE|各种属性更改事件|  
|EVENT_OBJECT_VALUECHANGE|<xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType> 已更改|  
|EVENT_SYSTEM_ALERT|无等效项|  
|EVENT_SYSTEM_CAPTUREEND|无等效项|  
|EVENT_SYSTEM_CAPTURESTART|无等效项|  
|EVENT_SYSTEM_CONTEXTHELPEND|无等效项|  
|EVENT_SYSTEM_CONTEXTHELPSTART|无等效项|  
|EVENT_SYSTEM_DIALOGEND|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent>|  
|EVENT_SYSTEM_DIALOGSTART|<xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent>|  
|EVENT_SYSTEM_DRAGDROPEND|无等效项|  
|EVENT_SYSTEM_DRAGDROPSTART|无等效项|  
|EVENT_SYSTEM_FOREGROUND|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_SYSTEM_MENUEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MENUSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MINIMIZEEND|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> 属性更改|  
|EVENT_SYSTEM_MINIMIZESTART|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> 属性更改|  
|EVENT_SYSTEM_MOVESIZEEND|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> 属性更改|  
|EVENT_SYSTEM_MOVESIZESTART|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty> 属性更改|  
|EVENT_SYSTEM_SCROLLINGEND|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> 或 <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> 属性更改|  
|EVENT_SYSTEM_SCROLLINGSTART|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> 或 <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> 属性更改|  
|EVENT_SYSTEM_SOUND|无等效项|  
|EVENT_SYSTEM_SWITCHEND|没有等效项，但 <xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent> 事件表示新的应用程序已收到焦点|  
|EVENT_SYSTEM_SWITCHSTART|无等效项|  
|无等效项|<xref:System.Windows.Automation.MultipleViewPattern.CurrentViewProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.HorizontallyScrollableProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.VerticallyScrollableProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.HorizontalViewSizeProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.ScrollPattern.VerticalViewSizeProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.TogglePattern.ToggleStateProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty> 属性更改|  
|无等效项|<xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent> 事件|  
|无等效项|<xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent>|  
  
<a name="Security_compare"></a>

## <a name="security"></a>安全性  

 某些 `IAccessible` 自定义应用场景需要包装一个基本 `IAccessible` 并对其调用。 这具有安全隐患，因为部分受信任的组件不应为代码路径上的中介。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 模型使提供程序不再需要通过其他提供程序代码调用。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 核心服务将进行所有必要的聚合。  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化基础知识](index.md)
