---
title: 使用 TreeWalker 在 UI 自动化元素之间导航
description: 请参阅代码示例，该示例演示如何使用 TreeWalker 类在 UI 自动化元素之间导航。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- classes, TreeWalker
- TreeWalker class
- elements, navigating among
- UI Automation, navigating among elements
ms.assetid: afcd21dc-2ffa-48c9-9332-51269f44b7e9
ms.openlocfilehash: e102e6ce258bef06d1fb44decfa1a3a27384e0bc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261210"
---
# <a name="navigate-among-ui-automation-elements-with-treewalker"></a>使用 TreeWalker 在 UI 自动化元素之间导航

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题包含示例代码，该代码演示如何 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 使用类在元素间导航 <xref:System.Windows.Automation.TreeWalker> 。  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Windows.Automation.TreeWalker.GetParent%2A> 遍历 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 树，直到它找到根元素或桌面。 紧靠下面的元素，它是指定元素的父窗口。  
  
 [!code-csharp[UIAFocusTracker_snip#102](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFocusTracker_snip/CSharp/FocusTracker.cs#102)]
 [!code-vb[UIAFocusTracker_snip#102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFocusTracker_snip/VisualBasic/FocusTracker.vb#102)]  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Windows.Automation.TreeWalker.GetFirstChild%2A> 和 <xref:System.Windows.Automation.TreeWalker.GetNextSibling%2A> 创建 <xref:System.Windows.Forms.TreeView> ，它显示 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 控件视图中的元素的整个子树以及已启用的子树。  
  
 [!code-csharp[UIAClient_snip#174](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#174)]
 [!code-vb[UIAClient_snip#174](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#174)]  
  
## <a name="see-also"></a>另请参阅

- [获取 UI 自动化元素](obtaining-ui-automation-elements.md)
