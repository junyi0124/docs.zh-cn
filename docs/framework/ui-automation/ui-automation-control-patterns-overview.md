---
title: UI 自动化控件模式概述
description: 请参阅 UI 自动化控件模式概述。 控件模式允许对控件的功能进行分类和公开，而不考虑类型或外观。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns
- UI Automation, control patterns
ms.assetid: cc229b33-234b-469b-ad60-f0254f32d45d
ms.openlocfilehash: e7d32be1532be7d673d8df4fe3f76c3d83fea913
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96278058"
---
# <a name="ui-automation-control-patterns-overview"></a>UI 自动化控件模式概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 此概述介绍 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 控件模式。 控件模式提供了一种方法，用于独立于控件类型或控件的外观对控件的功能进行分类和公开。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 使用控件模式表示常见的控件行为。 例如，你针对可被调用的控件（如按钮）使用调用控件模式并针对具有滚动条的控件（如列表框、列表视图或组合框）使用“滚动”控件模式。 由于每个控件模式表示单独的功能，它们可结合使用以描述由特定控件支持的完整功能组合。  
  
> [!NOTE]
> 聚合控件（由子控件生成，这些子控件提供由父级公开的功能的 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] ）应实现所有与每个子控件正常关联的控件模式。 反过来，不需要由这些子控件实现相同的这些控件模式。  
  
<a name="uiautomation_control_pattern_includes"></a>

## <a name="ui-automation-control-pattern-components"></a>UI 自动化控件模式组件  

 控件模式支持定义控件中可用的功能的离散件所需的方法、属性、事件和关系。  
  
- UI 自动化元素和其父级、子级和同级之间的关系描述 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树内该元素的结构。  
  
- 这些方法允许 UI 自动化客户端操作控件。  
  
- 属性和事件提供有关控件模式的功能的信息以及有关控件状态的信息。  
  
 控件模式与关联， [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 因为接口与组件对象模型相关， (COM) 对象。 在 COM 中，你可以查询对象以询问它支持什么接口，然后使用这些接口访问功能。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，UI 自动化客户端可以询问一个控件其所支持的控件模式，然后通过由支持的控件模式公开的属性、方法、事件和结构来与该控件进行交互。 例如，对于多行编辑框，UI 自动化提供程序实现 <xref:System.Windows.Automation.Provider.IScrollProvider>。 当客户端知道 <xref:System.Windows.Automation.AutomationElement> 支持 <xref:System.Windows.Automation.ScrollPattern> 控件模式时，它可以使用由该控件模式公开的属性、方法和事件来操作该控件或访问有关该控件的信息。  
  
<a name="uiautomation_control_pattern_client_provider"></a>

## <a name="ui-automation-providers-and-clients"></a>UI 自动化提供程序和客户端  

 UI 自动化提供程序实现控件模式以公开由控件支持的特定功能片段的适当行为。  
  
 UI 自动化客户端访问 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式类的方法和属性并使用它们获取有关 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]的信息或操作 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]。 这些控件模式类位于 <xref:System.Windows.Automation> 命名空间（例如， <xref:System.Windows.Automation.InvokePattern> 和 <xref:System.Windows.Automation.SelectionPattern>）。  
  
 客户端使用 <xref:System.Windows.Automation.AutomationElement> (（例如 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> 或 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType>) ）或公共语言运行时 (CLR) 访问器来访问模式的属性的方法 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 。 每个控件模式类都有一个字段成员 (例如， <xref:System.Windows.Automation.InvokePattern.Pattern?displayProperty=nameWithType> 或 <xref:System.Windows.Automation.SelectionPattern.Pattern?displayProperty=nameWithType>) 标识该控件模式，并且可以作为参数传递给 <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> 或 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> 以检索该模式 <xref:System.Windows.Automation.AutomationElement> 。  
  
<a name="uiautomation_control_patterns_dynamic"></a>

## <a name="dynamic-control-patterns"></a>动态控件模式  

 某些控件并不总是支持同一组控件模式。 当控件模式可用于 UI 自动化客户端时，则被视为受支持。 例如，多行编辑框仅在其包含多行可在其可视区域内显示的文本时才启用垂直滚动。 当删除了足够多的文本以便不再需要滚动时，滚动才被禁用。 就此示例而言，ScrollPattern 控件模式以动态方式受到支持，具体取决于该控件的当前状态（编辑框中的文本量）。  
  
<a name="Control_Pattern_Classes_and_Interfaces"></a>

## <a name="control-pattern-classes-and-interfaces"></a>控件模式类和接口  

 下表描述 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 该表还列出 UI 自动化客户端用于访问控件模式的类，以及由 UI 自动化提供程序用于实现它们的接口。  
  
|控件模式类|提供程序接口|描述|  
|---------------------------|------------------------|-----------------|  
|<xref:System.Windows.Automation.DockPattern>|<xref:System.Windows.Automation.Provider.IDockProvider>|用于可在停靠容器中停靠的控件。 例如，工具栏或工具调色板。|  
|<xref:System.Windows.Automation.ExpandCollapsePattern>|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|用于可展开或折叠的控件。 例如，应用程序中的菜单项，如 **“文件”** 菜单。|  
|<xref:System.Windows.Automation.GridPattern>|<xref:System.Windows.Automation.Provider.IGridProvider>|用于支持网格功能（如调整大小和移动到指定单元格）的控件。 例如，Windows 资源管理器中的大图标视图或 Microsoft Word 中没有标头的简单表格。|  
|<xref:System.Windows.Automation.GridItemPattern>|<xref:System.Windows.Automation.Provider.IGridItemProvider>|用于在网格内具有单元格的控件。 单个单元格应支持 GridItem 模式。 例如，Microsoft Windows 资源管理器详细信息视图中的每个单元。|  
|<xref:System.Windows.Automation.InvokePattern>|<xref:System.Windows.Automation.Provider.IInvokeProvider>|用于可被调用的控件，如按钮。|  
|<xref:System.Windows.Automation.MultipleViewPattern>|<xref:System.Windows.Automation.Provider.IMultipleViewProvider>|用于可在同一组信息、数据或子级的多个表示形式之间切换的控件。 例如，在列表视图控件中，数据可用于缩略图、磁贴、图标、列表或详细信息视图。|  
|<xref:System.Windows.Automation.RangeValuePattern>|<xref:System.Windows.Automation.Provider.IRangeValueProvider>|用于具有一系列可应用于该控件的值的控件。 例如，包含年份的微调框控件可能具有从 1900 到 2010 的年份范围，而表示月份的另一个微调框控件则会具有从 1 到 12 的月份范围。|  
|<xref:System.Windows.Automation.ScrollPattern>|<xref:System.Windows.Automation.Provider.IScrollProvider>|用于可滚动的控件。 例如，一个控件其所具有的滚动条在控件的可视区域中存在的信息超过了可被显示的信息时，便处于活动状态。|  
|<xref:System.Windows.Automation.ScrollItemPattern>|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|用于一种控件，该控件具有可滚动列表中的各个项。 例如，一个列表控件，该控件具有滚动列表中的各个项，如组合框控件。|  
|<xref:System.Windows.Automation.SelectionPattern>|<xref:System.Windows.Automation.Provider.ISelectionProvider>|用于选择容器控件。 例如，列表框和组合框。|  
|<xref:System.Windows.Automation.SelectionItemPattern>|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|用于选择容器控件中的各个项，如列表框和组合框。|  
|<xref:System.Windows.Automation.TablePattern>|<xref:System.Windows.Automation.Provider.ITableProvider>|用于具有网格以及标头信息的控件。 例如，Microsoft Excel 工作表。|  
|<xref:System.Windows.Automation.TableItemPattern>|<xref:System.Windows.Automation.Provider.ITableItemProvider>|用于表中的项。|  
|<xref:System.Windows.Automation.TextPattern>|<xref:System.Windows.Automation.Provider.ITextProvider>|用于可公开文本信息的编辑控件和文档。|  
|<xref:System.Windows.Automation.TogglePattern>|<xref:System.Windows.Automation.Provider.IToggleProvider>|用于在其中可切换状态的控件。 例如，复选框和可选中的菜单项。|  
|<xref:System.Windows.Automation.TransformPattern>|<xref:System.Windows.Automation.Provider.ITransformProvider>|用于可调整大小、移动和旋转的控件。 Transform 控件模式通常用于设计器、窗体、图形编辑器和绘图应用程序。|  
|<xref:System.Windows.Automation.ValuePattern>|<xref:System.Windows.Automation.Provider.IValueProvider>|允许客户端在不支持某个值范围的控件上获取或设置值。 例如，日期时间选择器。|  
|<xref:System.Windows.Automation.WindowPattern>|<xref:System.Windows.Automation.Provider.IWindowProvider>|向 Microsoft Windows 操作系统公开特定于窗口的信息（一种基本概念）。 Windows 控件的示例包括顶级应用程序窗口 (Microsoft Word、Microsoft Windows 资源管理器等) 、多文档界面 (MDI) 子窗口和对话框。|  
  
## <a name="see-also"></a>另请参阅

- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化客户端的控件模式映射](control-pattern-mapping-for-ui-automation-clients.md)
- [UI 自动化概述](ui-automation-overview.md)
- [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)
- [客户端的 UI 自动化事件](ui-automation-events-for-clients.md)
