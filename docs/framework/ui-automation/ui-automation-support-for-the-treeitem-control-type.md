---
title: UI 自动化对 TreeItem 控件类型的支持
description: 获取有关 UI 自动化对 TreeItem 控件类型的支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Tree Item
- Tree Item control type
- UI Automation, Tree Item control type
ms.assetid: 229f341a-477f-434e-b877-4db9973068eb
ms.openlocfilehash: 4ad53ee63d5e3ebb2cdd845a1258d191fa9fb521
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281258"
---
# <a name="ui-automation-support-for-the-treeitem-control-type"></a>UI 自动化对 TreeItem 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题提供有关对 TreeItem 控件类型的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持的信息。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 TreeItem 控件类型表示树容器中的节点。 各个节点可能包含其他节点，称为子节点。 以按展开或折叠的方式显示父节点或包含子节点的节点。  
  
 以下几节定义了 TreeItem 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有树项控件，无论是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与树项控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|TreeItem<br /><br /> -CheckBox (0 或 1) <br />-图像 (0 或 1) <br />-Button (0 或 1) <br />-TreeItem (0 个或更多) |TreeItem<br /><br /> -TreeItem (0 个或更多) |  
  
 树项控件可以有零个或多个 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中的树项子控件。 如果树项控件具有下面列出的控件模式中公开的范围以外的功能，则该控件应基于数据项控件类型。  
  
 折叠的树项将不会在控件视图或内容视图中显示，直到它们变为展开且可见（或可被滚动到视图中）。  
  
 控件视图可以包含控件的其他详细信息，包括关联的图像或按钮。 例如，大纲视图中的项可能包含图像，以及用于展开或折叠该大纲的按钮。 这些详细信息对象不会显示在内容视图中，因为信息已由父级树项表示。 滚动出屏幕的树项将显示在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图中，并且应将 <xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> 设置为 true。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与列表控件尤其相关。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|此属性必须返回项的位置，该项将导致项更改所选内容状态或获得焦点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|TreeItem|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|请参阅注释。|此属性设置为在树项控件滚动出屏幕时指示。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemTypeProperty>|请参阅注释。|如果树项控件使用可视化图标来指示这是特定类型的对象，则此属性必须受支持并指示该对象。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|树项控件自行进行标记。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“树项”|与 TreeItem 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|此属性公开为每个树项控件显示的文本。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出需要由列表控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式/模式属性|支持/值|注释|  
|---------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|依赖的对象|如果树项具有单独的、可操作的命令，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|是|可以展开或折叠所有树项。|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A>|展开、折叠或叶节点|当未展开或折叠时，树项将为叶节点。|  
|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|依赖的对象|如果树容器支持 Scroll 控件模式，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|依赖的对象|如果当用户返回到树容器时，有可能具有维护的活动选择，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider.SelectionContainer%2A>|是|此属性将公开容器内的所有项的同一个容器。|  
|<xref:System.Windows.Automation.Provider.IToggleProvider>|依赖的对象|如果树项具有关联的复选框，则实现此控件模式。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有树项控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [UI Automation Events Overview](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|注释|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemStatusProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty> 属性更改事件。|依赖的对象|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.TreeItem>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
