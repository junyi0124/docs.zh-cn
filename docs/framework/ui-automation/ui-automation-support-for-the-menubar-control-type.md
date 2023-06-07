---
title: UI 自动化对 MenuBar 控件类型的支持
description: 获取有关 UI 自动化对 MenuBar 控件类型的支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Menu Bar control type
- control types, Menu Bar
- Menu Bar control type
ms.assetid: c1202b21-c1f0-4560-853c-7b99bd73ad97
ms.openlocfilehash: 442dec22183164c11715357f48dd90c837cac6bb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96282972"
---
# <a name="ui-automation-support-for-the-menubar-control-type"></a>UI 自动化对 MenuBar 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件类型的 <xref:System.Windows.Automation.ControlType.MenuBar> 支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。  
  
 菜单栏控件是实现 MenuBar 控件类型的控件示例。 菜单栏为用户提供了一种激活应用程序中包含的命令和选项的方法。  
  
 以下几节定义了 MenuBar 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有列表控件，无论控件是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与菜单栏控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|MenuBar<br /><br /> -MenuItem (1 个或多个) <br />-其他控件 (0 个或多个) |MenuBar<br /><br /> -MenuItem (1 个或多个) <br />-其他控件 (0 个或多个) |  
  
 菜单栏控件可以在其结构中包含其他控件，如编辑控件和组合框。 这些附加控件与上面的控件和内容视图中列出的“其他控件”相对应。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出了值或定义与菜单栏控件密切相关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|此属性公开的值必须包括它所包含的所有控件。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|菜单栏控件不需要名称，除非在一个应用程序中具有多个菜单栏。 如果应用程序中有多个菜单栏，则此属性应该用于公开不同的名称，如“格式设置”或“大纲显示”。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|菜单栏控件绝对不会有标签。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|MenuBar|此值对于所有 UI 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“菜单栏”|与 MenuBar 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|菜单栏控件始终包含在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|菜单栏控件始终包含在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|请参阅注释。|此属性的值取决于是否可以在屏幕上查看控件。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.OrientationProperty>|依赖的对象|此属性将公开菜单栏控件是水平方向还是垂直方向。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|菜单栏控件是可以设定键盘焦点的控件，因为它们所包含的控件可以接收键盘焦点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|请参阅注释。|不存在菜单栏控件需要帮助文本的情况。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AcceleratorKeyProperty>|`Null`|菜单栏绝对没有快捷键。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>|“ALT”|按 ALT 键会始终将焦点移到应用程序中的菜单栏上。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出了需要由菜单栏控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|说明|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|依赖的对象|如果控件可以展开或折叠，则实现 <xref:System.Windows.Automation.Provider.IExpandCollapseProvider>。|  
|<xref:System.Windows.Automation.Provider.IDockProvider>|依赖的对象|如果控件可以停靠到屏幕的不同部分，则实现 <xref:System.Windows.Automation.Provider.IDockProvider>。|  
|<xref:System.Windows.Automation.Provider.ITransformProvider>|依赖的对象|如果可以对控件进行大小调整、旋转或移动，则控件必须实现 <xref:System.Windows.Automation.Provider.ITransformProvider>。|  
  
<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件  

 下表列出了需要由所有菜单栏控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [F:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持/值|注释|  
|---------------------------------------------------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> 属性更改事件。|依赖的对象|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.MenuBar>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
