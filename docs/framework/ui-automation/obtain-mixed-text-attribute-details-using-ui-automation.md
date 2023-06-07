---
title: 使用 UI 自动化获取混合文本特性的详细信息
description: 使用 .NET API 的 System.web 命名空间中的托管 UI 自动化类获取混合文本特性详细信息。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d0e4c005-abd1-42bb-92a4-5faf87097311
ms.openlocfilehash: ac6b212a8cb3f2f0cfa0d645aba2f0984ed8eb60
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274642"
---
# <a name="obtain-mixed-text-attribute-details-using-ui-automation"></a>使用 UI 自动化获取混合文本特性的详细信息

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题展示如何使用 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 从跨多个特性值的文本范围获取文本特性详细信息。 文本范围可以与文档、连续选择的文本、非连续选择的文本集合或文档的整个文本内容中的插入符号的当前位置相对应（或使选择范围退化）。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何从一个文本范围内获取 <xref:System.Windows.Automation.TextPattern.FontNameAttribute> ，其中 <xref:System.Windows.Automation.Text.TextPatternRange.GetAttributeValue%2A> 返回 <xref:System.Windows.Automation.TextPattern.MixedAttributeValue> 对象。  
  
[!code-csharp[FindText#RetrieveMixedAttributes](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#retrievemixedattributes)]
[!code-vb[FindText#RetrieveMixedAttributes](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#retrievemixedattributes)]  
  
 与 <xref:System.Windows.Automation.TextPattern> 类结合使用时， <xref:System.Windows.Automation.Text.TextPatternRange> 控件模式支持基本的文本特性、属性和方法。 对于不受 <xref:System.Windows.Automation.TextPattern> 或 <xref:System.Windows.Automation.Text.TextPatternRange>支持的特定于控件的功能， <xref:System.Windows.Automation.AutomationElement> 类为 UI 自动化客户端提供用于访问对应的本机对象模型的方法。  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化 TextPattern 概述](ui-automation-textpattern-overview.md)
- [使用 UI 自动化向文本框添加内容](add-content-to-a-text-box-using-ui-automation.md)
- [使用 UI 自动化查找和突出显示文本](find-and-highlight-text-using-ui-automation.md)
- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [使用 UI 自动化获取文本特性](obtain-text-attributes-using-ui-automation.md)
