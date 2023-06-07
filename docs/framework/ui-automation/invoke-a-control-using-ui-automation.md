---
title: 使用 UI 自动化调用控件
description: 使用 UI 自动化查找与特定属性条件相匹配的控件、创建 AutomationElement 和获取 InvokePattern，并使用控件上的 Invoke。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- invoking controls
- UI Automation, invoking controls
- controls, invoking
ms.assetid: 5ee2de3f-256c-43ec-b64c-62ace91f9983
ms.openlocfilehash: 7c6f5d26d16642f978fd79fd40701c240a53f16a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96277993"
---
# <a name="invoke-a-control-using-ui-automation"></a>使用 UI 自动化调用控件

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题演示如何执行以下任务：  
  
- 通过遍历目标应用程序 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图，查找与特定属性条件相匹配的控件。  
  
- 为每个控件创建 <xref:System.Windows.Automation.AutomationElement> 。  
  
- 从发现的任何支持 <xref:System.Windows.Automation.InvokePattern> 控件模式的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素中获取 <xref:System.Windows.Automation.InvokePattern> 对象。  
  
- 使用 <xref:System.Windows.Automation.InvokePattern.Invoke%2A> 以从客户端事件处理程序调用该控件。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> 类的 <xref:System.Windows.Automation.AutomationElement> 方法来生成 <xref:System.Windows.Automation.InvokePattern> 对象，并通过使用 <xref:System.Windows.Automation.InvokePattern.Invoke%2A> 方法调用控件。  
  
 [!code-csharp[InvokePatternApp#1100](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1100)]
 [!code-vb[InvokePatternApp#1100](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1100)]  
[!code-csharp[InvokePatternApp#1102](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1102)]
[!code-vb[InvokePatternApp#1102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1102)]  
  
## <a name="see-also"></a>另请参阅

- [InvokePattern、ExpandCollapsePattern 和 TogglePattern 示例](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
