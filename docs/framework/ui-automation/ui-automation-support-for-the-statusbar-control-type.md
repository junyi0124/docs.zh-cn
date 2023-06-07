---
title: UI 自动化对 StatusBar 控件类型的支持
description: 获取有关对状态栏控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- StatusBar control type
- UI Automation, Status Bar control type
- control types, Status Bar
ms.assetid: 48dee94a-5119-4939-a4c7-ffeaf794c732
ms.openlocfilehash: 04ef1e531a33486ad93d8f0540d8564ea8e9d222
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266995"
---
# <a name="ui-automation-support-for-the-statusbar-control-type"></a>UI 自动化对 StatusBar 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题提供有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 对于状态栏控件的支持信息。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 状态栏控件显示有关将在应用程序窗口中查看的对象和对象组件的信息，或应用程序内与该对象的操作相关的上下文信息。  
  
 以下几节定义状态栏控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有状态栏控件，无论是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述与状态栏控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|StatusBar<br /><br /> -Edit (0 个或多个) <br />-进度栏 (0 个或多个) <br />-Image (0 个或多个) <br />-Button (0 个或多个) |StatusBar<br /><br /> -Edit (0 个或多个) <br />-ProgressBar (0 个或多个) <br />-Image (0 个或多个) <br />-Button (0 个或多个) |  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出了 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与进度栏控件尤其相关。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性的详细信息，请参阅 [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|状态栏的边界矩形必须包含其中包含的所有控件。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|如果存在边界矩形，则受支持。 如果边界矩形中存在无法单击的点，而你要执行专门的命中测试，则重写并提供可单击的点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|状态栏控件不需要名称，除非应用程序中使用了多个状态栏。 在本例中，使用“Internet 状态”或“应用程序状态”等名称来区分每个状态栏。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|状态栏控件通常没有标签。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|StatusBar|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“状态栏”|与状态栏控件类型相对应的已本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|状态栏控件始终包含内容。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|状态栏控件始终是一个控件。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|依赖的对象|如果当前在屏幕上不可见，则状态栏控件将为此属性返回 True。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.OrientationProperty>|依赖的对象|控件方向的值：水平或垂直。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|False|不适用|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AcceleratorKeyProperty>|`Null`|状态栏不具有快捷键。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出需要由状态栏控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|说明|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridProvider>|可选|状态栏控件应支持“网格”控件模式，以便监视各个部分并可轻松引用信息。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出需要由所有状态栏控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [UI Automation Events Overview](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|说明|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.StatusBar>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
