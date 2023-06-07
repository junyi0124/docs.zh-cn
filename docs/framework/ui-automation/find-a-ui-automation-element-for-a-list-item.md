---
title: 查找列表项的 UI 自动化元素
description: 查看一个示例，该示例演示如何在已知项的索引时查找列表项的 UI 自动化元素。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- list items, finding elements for
- elements, finding for list items
- UI Automation, finding elements for List items
ms.assetid: c326ad2b-2144-4f64-ae4c-d850c74f95c5
ms.openlocfilehash: 95f6b558cc53b00701232f247f8de7f8c603e3ac
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276472"
---
# <a name="find-a-ui-automation-element-for-a-list-item"></a>查找列表项的 UI 自动化元素

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题演示如何 <xref:System.Windows.Automation.AutomationElement> 在已知项的索引时检索列表中的项。  
  
## <a name="example"></a>示例  

 下面的示例演示了两种方法，用于从列表中检索指定的项，一个使用 <xref:System.Windows.Automation.TreeWalker> ，另一个使用 <xref:System.Windows.Automation.AutomationElement.FindAll%2A> 。  
  
 第一种方法往往比 Win32 控件更快，但第二种方法的 Windows Presentation Foundation (WPF) 控件的速度更快。  
  
 [!code-csharp[UIAClient_snip#184](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#184)]
 [!code-vb[UIAClient_snip#184](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#184)]  
  
## <a name="see-also"></a>另请参阅

- [获取 UI 自动化元素](obtaining-ui-automation-elements.md)
