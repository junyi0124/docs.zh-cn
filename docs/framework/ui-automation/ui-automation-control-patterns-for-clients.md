---
title: 客户端的 UI 自动化控件模式
description: 阅读有关 UI 自动化客户端的控件模式的概述。 使用控件模式来访问有关用户界面 (UI) 的信息。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, control patterns for clients
- control patterns, UI Automation clients
ms.assetid: 571561d8-5f49-43a9-a054-87735194e013
ms.openlocfilehash: 3f58e9909b18681b3a0acf9332cc0c7c604e2002
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241962"
---
# <a name="ui-automation-control-patterns-for-clients"></a>客户端的 UI 自动化控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 此概述介绍 UI 自动化客户端的控件模式。 它包括有关 UI 自动化客户端如何使用控件模式访问 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)]相关信息的信息。  
  
 控件模式提供了一种方法，用于独立于控件类型或控件的外观对控件的功能进行分类和公开。 UI 自动化客户端可以检查 <xref:System.Windows.Automation.AutomationElement> 以确定哪些控件模式受支持并确保控件的行为。  
  
 有关控件模式的完整列表，请参阅 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)。  
  
<a name="uiautomation_getting_control_patterns"></a>

## <a name="getting-control-patterns"></a>获取控件模式  

 客户端通过调用 <xref:System.Windows.Automation.AutomationElement> 或 <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A?displayProperty=nameWithType> 从 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A?displayProperty=nameWithType>检索控件模式。  
  
 客户端可以使用 <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> 方法或单个 `IsPatternAvailable` 属性（例如 <xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty>）来确定一个模式或一组模式是否在 <xref:System.Windows.Automation.AutomationElement>上受支持。 但是，尝试获取控件模式并针对 `null` 引用进行测试比检查支持的属性并检索控件模式更高效，因为这样进行的跨进程调用更少。  
  
 以下示例演示如何从 <xref:System.Windows.Automation.TextPattern> 获取 <xref:System.Windows.Automation.AutomationElement>控件模式。  
  
 [!code-csharp[UIATextPattern_snip#1037](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#1037)]  
  
<a name="uiautomation_properties_on_control_patterns"></a>

## <a name="retrieving-properties-on-control-patterns"></a>检索控件模式上的属性  

 客户端可以通过调用 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> 或 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> 并将返回的对象强制转换为适当类型，来检索控件模式上的属性值。 有关属性的详细信息 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] ，请参阅 [客户端的 UI 自动化属性](ui-automation-properties-for-clients.md)。  
  
 除了 `GetPropertyValue` 方法之外，还可以通过公共语言运行时 (CLR) 访问器检索属性值以访问 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 模式上的属性。  
  
<a name="uiautomation_with_variable_patterns"></a>

## <a name="controls-with-variable-patterns"></a>具有可变模式的控件  

 某些控件类型支持不同的模式，具体取决于它们的状态或使用控件的方式。 可以具有可变模式的控件示例包括列表视图 (缩略图、磁贴、图标、列表、详细信息) 、Microsoft Excel 图表 (饼图、行、栏、带有公式的单元格值) 、Microsoft Word 文档区域 (普通、Web 版式、大纲、打印布局、打印预览) 和 Microsoft Windows Media Player 外观。  
  
 实现自定义控件类型的控件可以具有表示其功能所需的任何控件模式集。  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式](ui-automation-control-patterns.md)
- [UI 自动化文本模式](ui-automation-text-pattern.md)
- [使用 UI 自动化调用控件](invoke-a-control-using-ui-automation.md)
- [使用 UI 自动化获取复选框的切换状态](get-the-toggle-state-of-a-check-box-using-ui-automation.md)
- [UI 自动化客户端的控件模式映射](control-pattern-mapping-for-ui-automation-clients.md)
- [TextPattern 插入文本示例](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText)
- [TextPattern 搜索和选择示例](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FindText)
- [InvokePattern、ExpandCollapsePattern 和 TogglePattern 示例](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
