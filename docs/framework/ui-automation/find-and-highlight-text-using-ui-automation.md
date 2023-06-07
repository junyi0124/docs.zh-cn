---
title: 使用 UI 自动化查找和突出显示文本
description: 使用 UI 自动化查找和突出显示文本。 示例按顺序在文本控件内容中搜索和突出显示字符串的每个匹配项。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text, highlighting
- finding text
- text, finding
- UI automation, highlighting text
- UI automation, finding text
- highlighting text
ms.assetid: b77693f5-87bb-4b29-a297-05ff882e2044
ms.openlocfilehash: 7ae933bdf12c81e48371fa89ba5fc5cf5dd4731e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276459"
---
# <a name="find-and-highlight-text-using-ui-automation"></a>使用 UI 自动化查找和突出显示文本

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题演示如何使用在文本控件的内容中按顺序搜索和突出显示字符串的每个匹配项 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 。  
  
## <a name="example"></a>示例  

 下面的示例 <xref:System.Windows.Automation.TextPattern> 从文本控件获取对象。 <xref:System.Windows.Automation.Text.TextPatternRange>然后，使用此的属性创建一个对象，该对象表示整个文档的文本内容 <xref:System.Windows.Automation.TextPattern.DocumentRange%2A> <xref:System.Windows.Automation.TextPattern> 。 然后，将 <xref:System.Windows.Automation.Text.TextPatternRange> 为顺序搜索和突出显示功能创建其他两个对象。  
  
[!code-csharp[FindText#StartApp](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#startapp)]
[!code-vb[FindText#StartApp](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#startapp)]  
[!code-csharp[FindText#FindTextProvider](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#findtextprovider)]
[!code-vb[FindText#FindTextProvider](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#findtextprovider)]  
[!code-csharp[FindText#SearchTarget](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#searchtarget)]
[!code-vb[FindText#SearchTarget](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#searchtarget)]  
  
## <a name="see-also"></a>另请参阅

- [使用 UI 自动化查找和突出显示文本](find-and-highlight-text-using-ui-automation.md)
