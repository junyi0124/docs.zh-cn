---
title: UI 自动化对 DataGrid 控件类型的支持
description: 获取有关对 DataGrid 控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- Data Grid control type
- control types, Data Grid
- UI Automation, Data Grid control type
ms.assetid: a3db4a3f-feb5-4e5f-9b42-aae7fa816e8a
ms.openlocfilehash: 2efd8d5352da060baf019ff1b2b44df8d109503c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262809"
---
# <a name="ui-automation-support-for-the-datagrid-control-type"></a>UI 自动化对 DataGrid 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 对 DataGrid 控件类型的支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 `ControlType` 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 利用 DataGrid 控件类型，用户可以轻松地处理包含列中所显示元数据的项。 数据网格控件在行中显示项，在列中显示有关这些项的信息。 Microsoft Vista 资源管理器中的列表视图控件就是支持 DataGrid 控件类型的示例。  
  
 以下几节定义了 DataGrid 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有数据网格控件，无论控件是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。  
  
## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与数据网格控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 控件视图|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 内容视图|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|DataGrid<br /><br /> <ul><li>标头（0 个、1 个 或 2 个）<br /><br /> <ul><li>HeaderItem（列数或行数）</li></ul></li><li>DataItem（0 个或更多；可采用层次结构的形式构成）</li></ul>|DataGrid<br /><br /> -DataItem (0 个或更多;可在层次结构中进行结构) |  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出了值或定义与数据网格控件密切相关的属性。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|属性|值|注释|  
|--------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|如果存在边界矩形，则受支持。 如果边界矩形中存在无法单击的点，而你要执行专门的命中测试，则重写并提供可单击的点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|DataGrid|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|此属性的值必须始终为 True。 这意味着数据网格控件必须始终位于 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|此属性的值必须始终为 True。 这意味着数据网格控件必须始终位于 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|如果存在静态文本标签，则此属性必须公开对该控件的引用。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“数据网格”|与 DataGrid 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|数据网格控件通常从静态文本标签中获取其 `Name` 属性的值。 如果没有静态文本标签，则应用程序开发人员必须为 `Name` 属性赋值。 `Name` 属性的值决不应该是编辑控件的文本内容。|  
  
## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出了需要由所有数据网格控件支持的控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|注释|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridProvider>|是|数据网格控件本身始终支持 Grid 控件模式，因为它包含的项具有放置在网格中的元数据。|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|依赖的对象|是否能够滚动数据网格取决于内容以及滚动条是否存在。|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|依赖的对象|能否选择数据网格取决于内容。|  
|<xref:System.Windows.Automation.Provider.ITableProvider>|是|数据网格控件的子树内始终有标题，因此必须支持 Table 控件模式。|  
  
 数据网格容器内的数据项至少将支持：  
  
- Selection Item 控件模式（如果数据网格是可选择的）  
  
- Scroll Item 控件模式（如果数据网格是可滚动的）  
  
- Grid Item 控件模式  
  
- Table Item 控件模式  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有数据网格控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [UI Automation Events Overview](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|注释|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent>|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 属性更改事件。|依赖的对象|如果控件支持 Scroll 模式，则它必须支持此事件。|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|必选|无|  
  
## <a name="date-grid-control-type-example"></a>Date Grid 控件类型示例  

 下图阐释了实现 DataGrid 控件类型的列表视图控件。  
  
 ![具有两个数据项的 List View 控件图](./media/uiauto-data-grid-detailed.GIF "uiauto_data_grid_detailed")  
  
 下面显示了与 List View 控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图。 每个自动化元素的控件模式均显示在括号中。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 控件视图|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树 - 内容视图|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|<ul><li>DataGrid（Table、Grid、Selection）</li><li>标头<br /><br /> <ul><li>HeaderItem“名称”(Invoke)</li><li>HeaderItem“修改日期” (Invoke)</li><li>HeaderItem“大小” (Invoke)</li></ul></li><li>组 "Contoso" (TableItem、GridItem、SelectionItem、Table *、Grid \*) <br /><br /> <ul><li>DataItem "Accounts Receivable.doc" (SelectionItem、Invoke、TableItem \* 、GridItem \*) </li><li>DataItem "Accounts Payable.doc" (SelectionItem、Invoke、TableItem \* 、GridItem \*) </li></ul></li></ul>|<ul><li>DataGrid（Table、Grid、Selection）</li><li>组 "Contoso" (TableItem、GridItem、SelectionItem、Table *、Grid \*) <br /><br /> <ul><li>DataItem "Accounts Receivable.doc" (SelectionItem、Invoke、TableItem \* 、GridItem \*) </li><li>DataItem "Accounts Payable.doc" (SelectionItem、Invoke、TableItem \* 、GridItem \*) </li></ul></li></ul>|  
  
 \* 前面的示例显示了包含多个控件级别的 DataGrid。 Group（“Contoso”）控件包含两个 DataItem 控件（“Accounts Receivable.doc”和“Accounts Payable.doc”）。 DataGrid/GridItem 对不依赖于其他级别的对。 Group 下的 DataItem 控件还能以 ListItem 控件类型公开，使它们能够更清楚地呈现为可选择的对象，而不是简单的数据元素。 此示例不包括分组数据项的子元素。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.DataGrid>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
