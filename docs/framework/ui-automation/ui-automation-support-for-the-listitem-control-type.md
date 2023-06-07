---
title: UI 自动化对 ListItem 控件类型的支持
description: 获取有关对有类型的控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- control types, List
- List Item control type
- UI Automation, List Item control type
ms.assetid: 34f533bf-fc14-4e78-8fee-fb7107345fab
ms.openlocfilehash: 29e0dbc9ba99405dc2cca714efb6589ff427eccd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282959"
---
# <a name="ui-automation-support-for-the-listitem-control-type"></a>UI 自动化对 ListItem 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件类型的 <xref:System.Windows.Automation.ControlType.ListItem> 支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 列表项控件是实现 ListItem 控件类型的控件示例。  
  
 以下几节定义了 ListItem 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有列表控件，无论控件是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与列表项控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|ListItem<br /><br /> -Image (0 个或多个) <br />-文本 (0 个或多个) <br />-Edit (0 个或多个) |ListItem|  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中的列表项控件的子级必须始终为“0”。 如果该控件的结构为其他项包含在列表项之下，则它应遵循对 TreeItem 控件类型控件类型的 [UI 自动化支持](ui-automation-support-for-the-treeitem-control-type.md) 的要求。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与列表项控件密切相关。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|此属性的此值应包括图像的区域和列表项的文本内容。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|依赖的对象|如果列表控件具有可单击的点（可通过单击使列表获得焦点的点），则必须通过此属性公开该点。 如果列表控件完全由后代列表项覆盖，则它将引发 <xref:System.Windows.Automation.NoClickablePointException> ，以指示客户端必须向该列表控件中的项要求可单击的点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|列表项控件的名称属性的值来自项的文本内容。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|如果存在静态文本标签，则此属性必须公开对该控件的引用。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|ListItem|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“列表项”|与 ListItem 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|如果该容器可以接受键盘输入，则此属性的值应为 true。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|""|列表控件的帮助文本应说明为什么要求用户从选项列表中进行选择，帮助文本内容与工具提示所示的信息通常为同一类型。 例如，“选择一个项，以便为你的监视器设置显示分辨率。”|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemTypeProperty>|依赖的对象|此属性应为表示基础对象的列表项控件公开。 这些列表项控件通常有一个与控件关联的图标，用户将该控件与基础对象相关联。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|依赖的对象|此属性必须返回一个值，指示列表项当前是否滚动到实现 Scroll 控件模式的父容器内的视图中。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出了需要由列表项控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|注释|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|是|列表项控件必须实现此控件模式。 这允许列表项控件传达它们被选中的时间。|  
|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|依赖的对象|如果列表项包含在可滚动容器中，则必须实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IToggleProvider>|依赖的对象|如果列表项为可选中的列表项，并且操作不会执行所选内容状态更改，则必须实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|依赖的对象|如果可以操作项以显示或隐藏信息，则必须实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|依赖的对象|如果可以编辑项，则必须实现此控件模式。 更改列表项控件将导致对 <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>和 <xref:System.Windows.Automation.Provider.IValueProvider.Value%2A>的值进行更改。|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider>|依赖的对象|如果在列表容器内支持项之间的空间导航，并且容器按行和列排列，则必须实现 Grid Item 控件模式。|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|依赖的对象|如果项具有可以对其执行的命令（独立于所选内容），则必须实现此模式。 这通常是与双击列表项控件相关联的操作。 示例将从 Microsoft Windows 资源管理器中启动文档，或者在 Microsoft Windows Media Player 中播放音乐文件。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出了需要由所有列表项控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [F:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|说明|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|必须|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|必须|无|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemStatusProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.ListItem>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
