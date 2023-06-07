---
title: 使用 UI 自动化获取复选框的切换状态
description: 请参阅代码示例，该示例演示如何使用 Microsoft UI 自动化)  (如复选框来获取控件的切换状态。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, getting toggle states of check boxes
- check boxes, getting toggle states of
- getting, toggle states of check boxes
ms.assetid: 84fc31a3-175f-4e93-90a0-dd29d89b77ce
ms.openlocfilehash: 2ec0cc996461b13d0ddc3453188eeb3287a56be7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276433"
---
# <a name="get-the-toggle-state-of-a-check-box-using-ui-automation"></a>使用 UI 自动化获取复选框的切换状态

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题演示如何使用 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 来获取控件的切换状态。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> 类的方法 <xref:System.Windows.Automation.AutomationElement> <xref:System.Windows.Automation.TogglePattern> 从控件中获取对象，并返回其 <xref:System.Windows.Automation.ToggleState> 属性。  
  
 [!code-csharp[NavigatingWithTreeWalker#1200](../../../samples/snippets/csharp/VS_Snippets_Wpf/NavigatingWithTreeWalker/CSharp/ClientClass.cs#1200)]
 [!code-vb[NavigatingWithTreeWalker#1200](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/NavigatingWithTreeWalker/visualbasic/clientclass.vb#1200)]
