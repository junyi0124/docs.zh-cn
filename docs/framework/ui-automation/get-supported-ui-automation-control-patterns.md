---
title: 获取受支持的 UI 自动化控件模式
description: 阅读演示如何从 UI 自动化元素检索支持的控件模式对象的示例。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- control patterns, getting
- UI Automation, getting control patterns
- getting, control patterns
ms.assetid: 006c54c9-50bf-48d9-a855-9d62eb95603a
ms.openlocfilehash: 68abe272e91c40932aba5bcf99394c4a8f815c53
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276446"
---
# <a name="get-supported-ui-automation-control-patterns"></a>获取受支持的 UI 自动化控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题演示如何从 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素检索控件模式对象。  
  
### <a name="obtain-all-control-patterns"></a>获取所有控件模式  
  
1. 获取你对其控件模式感兴趣的 <xref:System.Windows.Automation.AutomationElement>。  
  
2. 调用 <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>，以便从该元素获取所有控件模式。  
  
> [!CAUTION]
> 强烈建议客户端不要使用 <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A>。 因为此方法针对每个现有的控件模式在内部调用 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A>，所以性能会受到严重影响。 如有可能，客户端应当针对相关的关键模式来调用 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A>。  
  
### <a name="obtain-a-specific-control-pattern"></a>获取特定的控件模式  
  
1. 获取你对其控件模式感兴趣的 <xref:System.Windows.Automation.AutomationElement>。  
  
2. 调用 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> 或 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> 以查询特定模式。 这两种方法非常相似，但是，如果没有找到该模式，<xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> 会引发异常，而 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> 会返回 `false`。  
  
## <a name="example"></a>示例  

 下面的示例检索某个列表项的 <xref:System.Windows.Automation.AutomationElement>，并从该元素获取一个 <xref:System.Windows.Automation.SelectionItemPattern>。  
  
 [!code-csharp[UIAClient_snip#103](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#103)]
 [!code-vb[UIAClient_snip#103](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#103)]  
  
## <a name="see-also"></a>另请参阅

- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
