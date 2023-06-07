---
title: UI 自动化对 Pane 控件类型的支持
description: 获取有关窗格控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Pane control type
- Pane control type
- control types, Pane
ms.assetid: 79761191-4449-4630-899c-9cbdb8867d3f
ms.openlocfilehash: 939d1e7bddf5fd97de067b00f88a488c97ef1b4c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282998"
---
# <a name="ui-automation-support-for-the-pane-control-type"></a>UI 自动化对 Pane 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍窗格控件类型的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 窗格控件类型用于表示框架或文档窗口中的对象。 用户可以在窗格控件之间以及当前窗格的内容中进行导航，但不能在不同窗格的项之间导航。 因此，窗格控件表示低于窗口或文档，但高于单个控件的分组级别。 用户通过按 TAB、F6、或 CTRL + TAB 在窗格间导航，具体取决于上下文。 窗格控件类型没有要求特定的键盘导航。  
  
 以下几节定义了窗格控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有列表控件，无论控件是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与窗格控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|窗格|窗格|  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与窗格控件尤其相关。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|此属性的值必须始终为清晰、简洁而有意义的标题。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|此属性公开窗格的可单击点，单击时可使窗格成为焦点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|窗格控件通常不具有静态标签。 如果没有静态文本标签，则它应通过此属性公开。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|窗格|此值对于所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“窗格”|与窗格控件类型相对应的已本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|窗格控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|窗格控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|""|窗格控件的帮助文本应解释框架的目的以及其如何与其他框架相关。 如果框架目的及联系与 `NameProperty`值相左，则需解释。 "|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>|请参阅注释。|如果特定的键组合让窗格成为焦点，则该信息应通过此属性公开。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出需要由所有窗格控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|说明|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITransformProvider>|依赖的对象|如果窗格控件可以移动、调整大小或在屏幕上旋转，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IWindowProvider>|从不|如果你需要实现此控件模式，则你的控件应基于 <xref:System.Windows.Automation.ControlType.Window> 控件类型。|  
|<xref:System.Windows.Automation.Provider.IDockProvider>|依赖的对象|如果窗格控件可以停靠，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|依赖的对象|如果窗格控件可以滚动，则实现此控件模式。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有窗格控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [F:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持/值|注释|  
|---------------------------------------------------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent>|从不|无|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent>|从不|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AsyncContentLoadedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowVisualStateProperty> 属性更改事件。|从不|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
  
<a name="Pane_Control_Type_Example"></a>

## <a name="pane-control-type-example"></a>窗格控件类型示例  

 下图阐释了实现窗格控件类型的控件。  
  
 ![具有两个窗格的小程序窗口屏幕快照](./media/uiauto-pane.GIF "uiauto_pane")  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 控件视图|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 内容视图|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|<ul><li>窗格</li><li>树（Scroll 模式）<br /><br /> <ul><li>TreeItem</li><li>窗格</li><li>编辑（Scroll 模式）</li></ul></li></ul>|-窗格<br />-Tree (滚动模式) <br />-TreeItem<br />- ...窗口<br />-Edit<br />- (滚动模式) |  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.Pane>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
