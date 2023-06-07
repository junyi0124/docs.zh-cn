---
title: UI 自动化对 ComboBox 控件类型的支持
description: 获取有关对 ComboBox 控件类型的 UI 自动化支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Combo Box
- UI Automation, Combo Box control type
- ComboBox controls
ms.assetid: bb321126-4770-41da-983a-67b7b89d45dd
ms.openlocfilehash: 11cc12dc78c74a294472ec7831548c50960147d0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260989"
---
# <a name="ui-automation-support-for-the-combobox-control-type"></a>UI 自动化对 ComboBox 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 对 ComboBox 控件类型的支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值、控件模式和 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件的特定准则。  
  
 组合框是与静态控件或编辑控件组合在一起的列表框，静态控件或编辑控件在组合框的列表框部分中显示了当前选定的项目。 控件的列表框部分要么将始终显示出来，要么只有当用户选择控件旁边的下拉箭头（下压按钮）时才会出现。 如果选择字段是一个编辑控件，则用户可以输入列表外的信息；否则，用户只能选择列表中的项目。  
  
 以下几节定义了 ComboBox 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有组合框控件，无论控件是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。  
  
<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构  

 下表描述了与组合框控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
|控件视图|内容视图|  
|------------------|------------------|  
|组合框<br /><br /> -Edit (0 或 1) <br />-List (1) <br />-列表项 (列表的子级;0到多个) <br />-Button (1) |组合框<br /><br /> -列表项 (0 到多个) |  
  
 只有在能够编辑组合框以接纳任何输入的情况下，组合框控件视图中的编辑控件才是必需的，“运行”对话框中的组合框也是如此。  
  
<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性  

 下表列出了值或定义与组合框控件密切相关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性的详细信息，请参阅 [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|如果存在边界矩形，则受支持。 如果边界矩形中存在无法单击的点，而你要执行专门的命中测试，则重写并提供可单击的点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|组合框|此值对于所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 框架均相同。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|请参阅注释。|组合框控件的帮助文本应解释为什么要求用户从组合框中选择一个选项。 文本与通过工具提示呈现的信息类似。 例如，“选择一个项来为你的监视器设置显示分辨率。”|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|组合框控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|组合框控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|组合框控件将公开选择容器中的一组项。 组合框控件可以接收键盘焦点，但 UI 自动化客户端在组合框上设置焦点时，组合框子树中的任何项都可以接收焦点。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|组合框控件通常具有此属性引用的静态文本标签。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“组合框”|与 ComboBox 控件类型相对应的本地化字符串。|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|组合框控件通常从静态文本控件获取其名称。|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>必需的 UI 自动化控件模式  

 下表列出了需要由所有组合框控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
|控件模式|支持|注释|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|是|组合框控件必须始终包含下拉按钮才能成为组合框。|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|是|显示组合框中的当前选择。 将委托组合框下面的列表框提供此支持。|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|依赖的对象|如果组合框能够接受任意文本值，则必须支持 Value 模式。 这种模式可以提供以编程方式设置组合框字符串内容的功能。 如果不支持 Value 模式，则表示用户必须从组合框子树内的列表项中进行选择。|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|从不|组合框上从不直接支持 Scroll 模式。 如果组合框内包括的列表框可以滚动，则支持此模式。 只有在屏幕上可以看到列表框时才支持此模式。|  
  
<a name="Required_Events"></a>

## <a name="required-events"></a>必需事件  

 下表列出了需要由所有组合框控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [F:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty](ui-automation-events-overview.md)。  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|注释|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty> 属性更改事件。|必选|无|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty> 属性更改事件。|依赖的对象|如果控件支持 Value 模式，则它必须支持此事件。|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.ComboBox>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
