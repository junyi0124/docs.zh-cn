---
title: UI 自动化对 Calendar 控件类型的支持
description: 获取有关 Calendar 控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Calendar control type
- Calendar control type
- control types, Calendar
ms.assetid: e91a7393-a7f9-4838-a1a6-857438b24bc9
ms.openlocfilehash: 66cb21b3285ddc64a268c43004fe2396df3af2d5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261002"
---
# <a name="ui-automation-support-for-the-calendar-control-type"></a>UI 自动化对 Calendar 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题提供有关针对 Calendar 控件类型的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持的信息。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值、控件模式和 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件的特定准则。  
  
 Calendar 控件允许用户轻松确定日期并选择其他日期。  
  
 以下几节定义了 Calendar 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有日历控件，无论是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与日历控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|日历<br /><br /> <ul><li>DataGrid<br /><br /> <ul><li>标头（0 个或 1 个）</li><li>HeaderItem（0 个或 7个；数量取决于列中显示了多少天）</li><li>ListItem（数量取决于显示了多少天）</li><li>Button（0 个或 2 个；用于对日历视图进行分页）</li></ul></li></ul>|日历<br /><br /> - (数量取决于显示多少天) |  
  
 Calendar 控件可在用户界面中表示为多种不同的形式。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中唯一确保存在的控件是数据网格、标头、标头项和列表项控件。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出了值或定义与日历控件密切相关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|如果存在边界矩形，则受支持。 如果边界矩形中存在无法单击的点，而你要执行专门的命中测试，则重写并提供可单击的点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|日历|此值对于所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|日历控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|日历控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|文档控件的标签。 通常使用文档的标题。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“日历”|与 Calendar 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|日历控件通常从当前日的日期中获取其名称。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出需要由所有日历控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式/模式属性|支持|注释|  
|---------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridProvider>|是|日历控件始终支持“网格”模式，因为月份中的日期是可以在空间中导航的项。|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|依赖的对象|大多数日历控件支持按页翻转视图。 建议使用“滚动”模式支持分页导航。|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|依赖的对象|大多数日历控件将特定日期、月份或年份保留为子元素的选定项。 某些日历具有多选功能和其他日历只能进行单选。|  
|<xref:System.Windows.Automation.Provider.ITableProvider>|是|日历控件在其子树内始终具有该星期所有日期的标头，因此必须支持表格模式。|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|否|Value 控件模式对于日历控件而言不是必要的，因为无法直接在控件上设置值。 如果特定日期与控件关联，则信息应由 Selection 控件模式提供。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有日历控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [UI Automation Events Overview](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|注释|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 属性更改事件。|依赖的对象|如果该控件支持 Scroll 控件模式，它必须支持此事件。|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|必选|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.Calendar>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
