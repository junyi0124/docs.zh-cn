---
title: UI 自动化对 Edit 控件类型的支持
description: 获取有关 UI 自动化对 Edit 控件类型的支持的信息。 了解必需的树结构、属性、控件模式和事件。
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Edit
- Edit control type
- UI Automation, Edit control type
ms.assetid: 6db9d231-c0a0-4e17-910e-ac80357f774f
ms.openlocfilehash: 6c404786d58cfcb4cc7dabd982eea33694b7cd0b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167935"
---
# <a name="ui-automation-support-for-the-edit-control-type"></a>UI 自动化对 Edit 控件类型的支持

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。

本主题介绍 Edit 控件类型的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持。 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，控件类型是一组条件，控件必须满足这些条件才能使用 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 属性。 这些条件包括针对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性值和控件模式的特定准则。

Edit 控件使用户能够查看和编辑简单文本行，无需丰富的格式设置支持。

以下几节定义了 Edit 控件类型必需的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树结构、属性、控件模式和事件。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]要求适用于所有编辑控件，无论是 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 、Win32 还是 Windows 窗体。

<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>必需的 UI 自动化树结构

下表描述了与 Edit 控件有关的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图和内容视图，以及每个视图中可包含的内容。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。

|控件视图|内容视图|
|------------------|------------------|
|编辑|编辑|

实现 Edit 控件类型的控件在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中始终有零个滚动条，因为它是单行控件。 在某些布局方案中，单个文本行可能会换行。 Edit 控件类型最适用于保留少量的可编辑或可选文本。

<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>必需的 UI 自动化属性

下表列出 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，这些属性的值或定义与编辑控件尤其相关。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性的详细信息，请参阅 [UI Automation Properties for Clients](ui-automation-properties-for-clients.md)。

|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性|值|注释|
|------------------------------------------------------------------------------------|-----------|-----------|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|请参阅注释。|此属性的值在应用程序的所有控件中都必须保持唯一。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|请参阅注释。|包含整个控件的最外层矩形。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|请参阅注释。|编辑控件必须具有可单击的点，当用户鼠标单击那里时，将赋予该控件编辑部分输入焦点。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|请参阅注释。|如果该控件可以接收键盘焦点，则它必须支持此属性。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|请参阅注释。|编辑控件的名称通常是从静态文本标签生成的。 如果没有静态文本标签，则 `Name` 的属性值必须由应用程序开发人员分配。 `Name` 属性决不应包含编辑控件的文本内容。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|请参阅注释。|如果存在与控件关联的静态文本标签，则该属性必须公开对该控件的引用。 如果文本控件是另一个控件的子组件，则该文本控件没有 `LabeledBy` 属性集。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|编辑|此值对于所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 框架均相同。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|“编辑”|与 Edit 控件类型相对应的本地化字符串。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|编辑控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图中。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|编辑控件始终包括在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsPasswordProperty>|请参阅注释。|包含密码的编辑控件必须设置为 true。 如果一个编辑控件中确实包含密码内容，则此属性可由屏幕阅读器使用以确定用户键入时，是否应显示击键。|

<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns-and-properties"></a>必需的 UI 自动化控件模式和属性

下表列出需要由所有编辑控件支持的控件模式。 有关控件模式的详细信息，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。

|控件模式/控件模式属性|支持/值|注释|
|-----------------------------------------------|--------------------|-----------|
|<xref:System.Windows.Automation.Provider.ITextProvider>|依赖的对象|编辑控件应支持 Text 控件模式，因为详细的文本信息应始终可供客户端使用。|
|<xref:System.Windows.Automation.Provider.IValueProvider>|依赖的对象|采用字符串的所有编辑控件都必须公开 Value 模式。|
|<xref:System.Windows.Automation.Provider.IValueProvider.IsReadOnly%2A>|请参阅注释。|必须设置此属性，以指示控件能以编程方式设置值或对用户可编辑。|
|<xref:System.Windows.Automation.Provider.IValueProvider.Value%2A>|请参阅注释。|此属性将返回编辑控件的文本内容。 如果 `IsPasswordProperty` 设置为 `true`，则请求时此属性必须引发 `InvalidOperationException` 。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider>|依赖的对象|采用数值范围的所有编辑控件都必须公开 Range Value 控件模式。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider.Minimum%2A>|请参阅注释。|此属性必须是编辑控件的内容可以设置的最小值。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider.Maximum%2A>|请参阅注释。|此属性必须是编辑控件的内容可以设置的最大值。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider.SmallChange%2A>|请参阅注释。|此属性必须指示该值可以设置的小数位数。 如果编辑只能采用整数，则 `SmallChangeProperty` 必须为 1。 如果编辑的范围是 1.0 到 2.0，则 `SmallChangeProperty` 必须为 0.1。 如果编辑控件采用的范围是 1.00 到 2.00，则 `SmallChangeProperty` 必须为 0.001。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider.LargeChange%2A>|`Null`|此属性不需要在编辑控件上公开。|
|<xref:System.Windows.Automation.Provider.IRangeValueProvider.Value%2A>|请参阅注释。|此属性将指示编辑控件的数字内容。 当 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 客户端设置 `Minimum` 和 `Maximum` 属性所指定范围内的更准确值时，Value 属性将自动为舍入最接近的接受值。|

<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>必需的 UI 自动化事件

下表列出需要由所有编辑控件支持的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件。 有关事件的详细信息，请参阅 [UI Automation Events Overview](ui-automation-events-overview.md)。

|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件|支持|注释|
|---------------------------------------------------------------------------------|-------------|-----------|
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|必需|无|
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent>|必须|无|
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent>|必须|无|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 属性更改事件。|必需|无|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 属性更改事件。|必需|无|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 属性更改事件。|必需|无|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> 属性更改事件。|必需|无|
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty> 属性更改事件。|依赖的对象|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 属性更改事件。|从不|无|
|<xref:System.Windows.Automation.RangeValuePatternIdentifiers.ValueProperty> 属性更改事件。|依赖的对象|如果该控件支持 Range Value 控件模式，它必须支持此事件。|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|必需|无|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|必须|无|

## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.ControlType.Edit>
- [UI 自动化控件类型概述](ui-automation-control-types-overview.md)
- [UI 自动化概述](ui-automation-overview.md)
