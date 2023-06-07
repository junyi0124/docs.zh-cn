---
title: UI 自动化对 List 控件类型的支持
description: 获取有关对 List 控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- control types, List
- List control type
- UI Automation, List control type
ms.assetid: 0e959fcb-50f2-413b-948d-7167d279bc11
ms.openlocfilehash: b56856e3157ad4b770173cf0b34c7e092ffff075
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247234"
---
# <a name="ui-automation-support-for-the-list-control-type"></a>UI 自动化对 List 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍 List 控件类型的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 List 控件类型提供一种方法来组织平面组或项组并允许用户选择一个或多个这些项。 List 控件类型对它可能包含哪些类型的子元素具有宽松的限制。 这使 UI 自动化提供程序可以支持选择容器中的已知元素。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]以下各节中的要求适用于实现 List 控件类型的所有控件，无论是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。 List 容器控件是实现 List 控件类型的控件示例。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与列表控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的两种视图，以及说明了每种视图中可包含的内容。 控件视图只包含作为控件的元素，而内容视图从树中删除冗余信息。 例如，用来标记组合框的文本控件将作为 `ComboBox NameProperty`公开。 因为文本控件已通过控件视图以这种方式公开，所以没有必要让它公开两次 ；因此将它从内容视图中删除。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|包含对应于控件的元素。|从树中删除冗余信息，以便辅助技术使用最小部分的对最终用户有意义的信息。|  
|列出<br /><br /> -DataItem (0 个或更多) <br />- (0 个或多个) <br />-Group (0 个或多个) <br />-滚动条 (0、1或 2) |列出<br /><br /> -DataItem (0 个或更多) <br />- (0 个或多个) <br />-Group (0 个或多个) |  
  
 实现 List 控件类型（如列表控件）的控件的控件视图组成如下：  
  
- 列表控件中的零个或多个项 (项可以基于) 的列表项或数据项控件类型。
  
- 列表控件中的零个或多个组控件。
  
- 零个、一个或两个滚动条控件。
  
实现 List 控件类型（如列表控件）的控件的内容视图组成如下：  
  
- 列表控件中的零个或多个项 (项可以基于) 的列表项或数据项控件类型。
  
- 列表控件中的零个或多个组。

列表控件不得具有除了组合在一起外的层次结构关系。 如果这些项在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中具有子项，则列表容器应基于 Tree 控件类型。  
  
 在列表控件中可选择的项都可从列表控件的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中的子代获取。 列表控件中的所有项都必须都属于同一个选择组。 在列表中的可选择项应作为 ListItem（而不是 DataItem）控件类型公开。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与列表控件尤其相关。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|如果列表控件具有可单击的点（可通过单击使列表获得焦点的点），则必须通过此属性公开该点。<br /><br /> 如果该属性的值 `IsOffScreen` 为 true，则 <xref:System.Windows.Automation.NoClickablePointException> 将引发。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|列表控件的 Name 属性的值应传达用户需要从其中选择的选项类别。 此属性通常从静态文本标签获取其名称。 如果没有静态文本标签，则应用程序开发人员必须公开 Name 属性的值。<br /><br /> 只有在另一个控件的子树内使用控件时，列表控件才不要求此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|如果存在静态文本标签，则此属性必须公开对该控件的引用。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|列出|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“列表”|与 List 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|列表控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|如果该容器可以接受键盘输入，则此属性的值应为 true。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|请参阅注释。|列表控件的帮助文本应解释为什么要求用户从选项列表中进行选择。 例如，“从此列表中选择项将会设置显示监视器的分辨率。”|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns-and-properties"></a>必需的 UI 自动化控件模式和属性  

 下表列出需要由列表控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式/模式属性|支持/值|注释|  
|---------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|必须|当选定状态维持在控件中包含的项之间时，支持对 List 控件类型的所有控件必须实现 `ISelectionProvider` 。 如果容器内的项不可选，则必须使用 Group 控件类型。|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.IsSelectionRequired%2A>|依赖的对象|List 控件并不总是要求选择一个项。|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.CanSelectMultiple%2A>|依赖的对象|List 控件可以是单选或多选容器。|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|依赖的对象|如果容器中的项可滚动，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.IGridProvider>|依赖的对象|当网格导航需要在项上以逐个项的方式可用，则实现此模式。|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider>|依赖的对象|如果该控件可以支持容器中的项的多个视图，则实现此控件模式。|  
|<xref:System.Windows.Automation.Provider.ITableProvider>|从不|List 控件类型从不支持`ITableProvider` 。 如果控件应该支持此控件模式，该该控件应基于的数据网格控件类型。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有文本控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [F:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持/值|注释|  
|---------------------------------------------------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.List>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
