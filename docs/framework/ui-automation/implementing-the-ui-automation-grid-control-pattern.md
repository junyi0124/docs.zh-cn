---
title: 实现 UI 自动化 Grid 控件模式
description: 了解在 UI 自动化中实现 GridPattern grid 控件模式的准则和约定。 了解如何实现 IGridProvider 接口。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, grid
- grid control pattern
- UI Automation, grid control pattern
ms.assetid: 234d11a0-7ce7-4309-8989-2f4720e02f78
ms.openlocfilehash: 2290fd91c8eee0ab969eef2827d3c7440ef21e20
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274876"
---
# <a name="implementing-the-ui-automation-grid-control-pattern"></a>实现 UI 自动化 Grid 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍实现 <xref:System.Windows.Automation.Provider.IGridProvider>的准则和约定，包括有关属性、方法和事件的信息。 本概述的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.GridPattern> 控件模式用于支持作为子元素集合的容器的控件。 此元素的子元素必须实现 <xref:System.Windows.Automation.Provider.IGridItemProvider> ，并且在可以按行和列进行遍历的二维逻辑坐标系统中进行组织。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现 Grid 控件模式时，请注意以下准则和约定：  
  
- 网格坐标从零开始，左上角单元格（或右上角单元格，具体取决于区域设置）的坐标为 (0, 0)。  
  
- 如果某个单元格为空，必须仍返回 UI 自动化元素以便支持该单元格的 <xref:System.Windows.Automation.Provider.IGridItemProvider.ContainingGrid%2A> 属性。 当网格中的子元素的布局类似于未对齐的数组时，这是可能的（请参阅下面的示例）。  
  
 ![显示未对齐布局的 Windows 资源管理器视图。](./media/uia-gridpattern-ragged-array.PNG "UIA_GridPattern_Ragged_Array")  
坐标为空的 Grid 控件的示例  
  
- 只有一项的网格仍需要实现 <xref:System.Windows.Automation.Provider.IGridProvider> （如果它逻辑上被视为网格）。 网格中的子项数并不重要。  
  
- 隐藏的行和列（具体取决于提供程序实现）可能会在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中加载，因此将会反映在 <xref:System.Windows.Automation.GridPattern.GridPatternInformation.RowCount%2A> 和 <xref:System.Windows.Automation.GridPattern.GridPatternInformation.ColumnCount%2A> 属性中。 如果尚未加载隐藏的行和列，则不应对它们进行计数。  
  
- <xref:System.Windows.Automation.Provider.IGridProvider> 不会启用网格的主动操作；必须实现 <xref:System.Windows.Automation.Provider.ITransformProvider> 以启用此功能。  
  
- 使用 <xref:System.Windows.Automation.StructureChangedEventHandler> 来侦听对网格的结构和布局更改（如已添加、删除或合并的单元格）。  
  
- 使用 <xref:System.Windows.Automation.AutomationFocusChangedEventHandler> 来跟踪遍历的网格的项或单元格。  
  
<a name="Required_Members_for_IGridProvider"></a>

## <a name="required-members-for-igridprovider"></a>IGridProvider 必需的成员  

 实现 IGridProvider 接口需要以下属性和方法。  
  
|必需的成员|类型|注释|  
|----------------------|----------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridProvider.RowCount%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IGridProvider.ColumnCount%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IGridProvider.GetItem%2A>|方法|无|  
  
 没有与此控件模式关联的事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|例外类型|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentOutOfRangeException>|<xref:System.Windows.Automation.Provider.IGridProvider.GetItem%2A><br /><br /> -如果请求的行坐标大于 <xref:System.Windows.Automation.Provider.IGridProvider.RowCount%2A> 或列坐标大于，则为 <xref:System.Windows.Automation.Provider.IGridProvider.ColumnCount%2A> 。|  
|<xref:System.ArgumentOutOfRangeException>|<xref:System.Windows.Automation.Provider.IGridProvider.GetItem%2A><br /><br /> -如果请求的行坐标或列坐标都小于零。|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [实现 UI 自动化 GridItem 控件模式](implementing-the-ui-automation-griditem-control-pattern.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
