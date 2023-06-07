---
title: UI 自动化事件概述
description: 请参阅 Microsoft UI 自动化事件通知概述。 查看事件类型、UI 自动化事件标识符和 UI 自动化事件参数。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- UI Automation, events
- clients, UI Automation
- events, UI Automation
- providers, UI Automation
- UI Automation, clients
ms.assetid: 69eebd8b-39ed-40e7-93cc-4457c4caf746
ms.openlocfilehash: 8a97c4bfbe361eafc11cd920b032424f67c3a5fb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240441"
---
# <a name="ui-automation-events-overview"></a>UI 自动化事件概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 事件通知是屏幕读取器和屏幕放大器等辅助技术的一项重要功能。 这些 UI 自动化客户端跟踪由 UI 自动化提供程序引发的事件（当 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 中发生一些事情时），并使用这些信息通知最终用户。  
  
 通过允许提供程序应用程序有选择地引发事件来提高效率，这具体取决于是否有客户端订阅了这些事件，或者如果没有客户端在侦听任意事件，则不会引发任何事件。  
  
<a name="Types_of_Events"></a>

## <a name="types-of-events"></a>事件类型  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件分为以下几类：  
  
|事件|描述|  
|-----------|-----------------|  
|属性更改|当 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素上的某个属性或控件模式更改时引发。 例如，如果客户端需要监视应用程序的复选框控件，它可以注册来侦听 <xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> 属性上的属性更改事件。 选中或取消选中该复选框控件时，提供程序会引发事件且客户端会采取必要的操作。|  
|元素操作|当来自最终用户或编程活动的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 结果出现更改时引发；例如，单击或通过 <xref:System.Windows.Automation.InvokePattern>调用一个按钮。|  
|结构更改|当 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的结构更改时引发。 当新的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项在桌面上变成可见、隐藏或被删除时，结构发生变化。|  
|全局桌面更改|当与客户端相关的的全局操作发生时引发，例如当焦点从一个元素转换到另一个元素、或窗口关闭时。|  
  
 一些事件不一定意味着 UI 状态已更改。 例如，当用户通过 tab 键移动到文本输入字段，然后单击一个按钮更新该字段时，如果用户实际上并未更改文本，则会引发 `TextChangedEvent` 。 处理事件时，客户端应用程序在执行操作之前可能有必要检查是否实际发生了任何更改。  
  
 即使 UI 的状态未更改，也可能会引发以下事件。  
  
- `AutomationPropertyChangedEvent` （取决于已更改的属性）  
  
- `ElementSelectedEvent`  
  
- `InvalidatedEvent`  
  
- `TextChangedEvent`  
  
<a name="UI_Automation_Event_Identifiers"></a>

## <a name="ui-automation-event-identifiers"></a>UI 自动化事件标识符  

 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 事件由 <xref:System.Windows.Automation.AutomationEvent> 对象标识。 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> 属性包含唯一标识这类事件的值。  
  
 下表给出了 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> 的可能值，以及用于事件参数的类型。 请注意，由客户端和提供程序使用的标识符都是来自不同类的相同命名的字段。  
  
|客户端标识符|提供程序标识符|事件参数类型|  
|-----------------------|-------------------------|--------------------------|  
|<xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|  
|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPattern.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePattern.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.StructureChangedEventArgs>|  
|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowClosedEventArgs>|  
  
<a name="UI_Automation_Event_Arguments"></a>

## <a name="ui-automation-event-arguments"></a>UI 自动化事件参数  

 下列类能封装事件参数。  
  
|实例|描述|  
|-----------|-----------------|  
|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|包含有关内容异步加载的信息，包括加载已完成的百分比的信息。|  
|<xref:System.Windows.Automation.AutomationEventArgs>|包含有关无需额外数据的简单事件的信息。|  
|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|包含有关输入焦点从一个元素到另一个元素的变化的信息。 此类型的事件是由 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 系统引发，而不是由提供程序引发。|  
|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|包含有关元素或控件模式的属性值的变化的信息。|  
|<xref:System.Windows.Automation.StructureChangedEventArgs>|包含有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的变化的信息。|  
|<xref:System.Windows.Automation.WindowClosedEventArgs>|包含有关窗口关闭的信息。|  
  
 所有事件参数类都包含一个 <xref:System.Windows.Automation.AutomationEventArgs.EventId%2A> 成员。 此标识符封装在一个 <xref:System.Windows.Automation.AutomationEvent>中。  
  
 提供程序可从 <xref:System.Windows.Automation.AutomationEvent> 中的字段和控件模式标识符类（如 <xref:System.Windows.Automation.AutomationElementIdentifiers> ）获得用于标识事件的 <xref:System.Windows.Automation.DockPatternIdentifiers>对象。 应用程序客户端可从 <xref:System.Windows.Automation.AutomationElement> 中的字段和控件模式类（如 <xref:System.Windows.Automation.DockPattern>）获得等效的字段。  
  
 有关事件标识符的列表，请参阅 [UI Automation Events for Clients](ui-automation-events-for-clients.md)。  
  
## <a name="see-also"></a>另请参阅

- [客户端的 UI 自动化事件](ui-automation-events-for-clients.md)
- [服务器端 UI 自动化提供程序的实现](server-side-ui-automation-provider-implementation.md)
- [订阅 UI 自动化事件](subscribe-to-ui-automation-events.md)
