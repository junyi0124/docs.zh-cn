---
title: UI 自动化控件类型概述
description: 阅读 UI 自动化控件类型的概述，这些控件类型是已知的标识符，可用于指示元素所表示的控件类型。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, control types
- control types, UI Automation
ms.assetid: 75159ef8-bd43-4d13-acb7-1f1fe9253160
ms.openlocfilehash: 851d509e719afb658971ea5f6fc2f8fdd6bd2cf7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261340"
---
# <a name="ui-automation-control-types-overview"></a>UI 自动化控件类型概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 控件类型是已知的标识符，可用于指示特定元素显示的控件类型，例如组合框或按钮。  
  
 具有已知标识符便于辅助技术设备确定 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 中可用的控件的类型以及和控件交互的方式。  
  
<a name="UI_Automation_Control_Type_Requisites"></a>

## <a name="ui-automation-control-type-requisites"></a>UI 自动化控件类型必备条件  

 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 控件类型提供一组提供程序必须满足的条件。 满足这些条件时，控件可以使用特定的控件类型名称。 每个控件类型对以下内容设有条件：  
  
- [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式 — 必须支持哪些控件模式、哪个控件模式是可选的以及控件不得支持哪些控件模式。  
  
- [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值 — 支持哪些属性值。  
  
- [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构 — 控件必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构。  
  
 当控件满足特定控件类型的条件时， <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> 属性值将指示该控件类型。  
  
<a name="Current_UI_Automation_Control_Types"></a>

## <a name="current-ui-automation-control-types"></a>当前的 UI 自动化控件类型  

 以下列表包含当前 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 控制类型集：  
  
- [UI 自动化对 Button 控件类型的支持](ui-automation-support-for-the-button-control-type.md)  
  
- [UI 自动化对 Calendar 控件类型的支持](ui-automation-support-for-the-calendar-control-type.md)  
  
- [UI 自动化对 CheckBox 控件类型的支持](ui-automation-support-for-the-checkbox-control-type.md)  
  
- [UI 自动化对 ComboBox 控件类型的支持](ui-automation-support-for-the-combobox-control-type.md)  
  
- [UI 自动化对 DataGrid 控件类型的支持](ui-automation-support-for-the-datagrid-control-type.md)  
  
- [UI 自动化对 DataItem 控件类型的支持](ui-automation-support-for-the-dataitem-control-type.md)  
  
- [UI 自动化对 Document 控件类型的支持](ui-automation-support-for-the-document-control-type.md)  
  
- [UI 自动化对 Edit 控件类型的支持](ui-automation-support-for-the-edit-control-type.md)  
  
- [UI 自动化对 Group 控件类型的支持](ui-automation-support-for-the-group-control-type.md)  
  
- [UI 自动化对 Header 控件类型的支持](ui-automation-support-for-the-header-control-type.md)  
  
- [UI 自动化对 HeaderItem 控件类型的支持](ui-automation-support-for-the-headeritem-control-type.md)  
  
- [UI 自动化对 Hyperlink 控件类型的支持](ui-automation-support-for-the-hyperlink-control-type.md)  
  
- [UI 自动化对 Image 控件类型的支持](ui-automation-support-for-the-image-control-type.md)  
  
- [UI 自动化对 List 控件类型的支持](ui-automation-support-for-the-list-control-type.md)  
  
- [UI 自动化对 ListItem 控件类型的支持](ui-automation-support-for-the-listitem-control-type.md)  
  
- [UI 自动化对 Menu 控件类型的支持](ui-automation-support-for-the-menu-control-type.md)  
  
- [UI 自动化对 MenuBar 控件类型的支持](ui-automation-support-for-the-menubar-control-type.md)  
  
- [UI 自动化对 MenuItem 控件类型的支持](ui-automation-support-for-the-menuitem-control-type.md)  
  
- [UI 自动化对 Pane 控件类型的支持](ui-automation-support-for-the-pane-control-type.md)  
  
- [UI 自动化对 ProgressBar 控件类型的支持](ui-automation-support-for-the-progressbar-control-type.md)  
  
- [UI 自动化对 RadioButton 控件类型的支持](ui-automation-support-for-the-radiobutton-control-type.md)  
  
- [UI 自动化对 ScrollBar 控件类型的支持](ui-automation-support-for-the-scrollbar-control-type.md)  
  
- [UI 自动化对 Separator 控件类型的支持](ui-automation-support-for-the-separator-control-type.md)  
  
- [UI 自动化对 Slider 控件类型的支持](ui-automation-support-for-the-slider-control-type.md)  
  
- [UI 自动化对 Spinner 控件类型的支持](ui-automation-support-for-the-spinner-control-type.md)  
  
- [UI 自动化对 SplitButton 控件类型的支持](ui-automation-support-for-the-splitbutton-control-type.md)  
  
- [UI 自动化对 StatusBar 控件类型的支持](ui-automation-support-for-the-statusbar-control-type.md)  
  
- [UI 自动化对 Tab 控件类型的支持](ui-automation-support-for-the-tab-control-type.md)  
  
- [UI 自动化对 TabItem 控件类型的支持](ui-automation-support-for-the-tabitem-control-type.md)  
  
- [UI 自动化对 Table 控件类型的支持](ui-automation-support-for-the-table-control-type.md)  
  
- [UI 自动化对 Text 控件类型的支持](ui-automation-support-for-the-text-control-type.md)  
  
- [UI 自动化对 Thumb 控件类型的支持](ui-automation-support-for-the-thumb-control-type.md)  
  
- [UI 自动化对 TitleBar 控件类型的支持](ui-automation-support-for-the-titlebar-control-type.md)  
  
- [UI 自动化对 ToolBar 控件类型的支持](ui-automation-support-for-the-toolbar-control-type.md)  
  
- [UI 自动化对 ToolTip 控件类型的支持](ui-automation-support-for-the-tooltip-control-type.md)  
  
- [UI 自动化对 Tree 控件类型的支持](ui-automation-support-for-the-tree-control-type.md)  
  
- [UI 自动化对 TreeItem 控件类型的支持](ui-automation-support-for-the-treeitem-control-type.md)  
  
- [UI 自动化对 Window 控件类型的支持](ui-automation-support-for-the-window-control-type.md)  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType>
