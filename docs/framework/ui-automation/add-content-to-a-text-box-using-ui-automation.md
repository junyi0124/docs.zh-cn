---
title: 使用 UI 自动化向文本框添加内容
description: 请参阅如何在 .NET 中使用 Microsoft UI Automation 将内容添加到单行文本框中的示例。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- adding content to text boxes
- text boxes, adding content
- UI Automation, adding content to text boxes
ms.assetid: 8bdd1a73-1ecb-4a05-a891-a7827ebb767f
ms.openlocfilehash: 9108cb586700474f7f855751000944212a3cef29
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235722"
---
# <a name="add-content-to-a-text-box-using-ui-automation"></a>使用 UI 自动化向文本框添加内容

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题包含演示如何使用 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 将文本插入单行文本框的示例代码。 为多行控件和丰富文本控件提供了一个替代方法，这些控件 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 不适用。 出于比较目的，此示例还演示了如何使用 Win32 方法来实现相同的结果。  
  
## <a name="example"></a>示例  

 下面的示例逐步介绍目标应用程序中的一系列文本控件。 对每个文本控件进行测试，以查看是否 <xref:System.Windows.Automation.ValuePattern> 可以使用方法从对象获取对象 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> 。 如果文本控件支持，则 <xref:System.Windows.Automation.ValuePattern> <xref:System.Windows.Automation.ValuePattern.SetValue%2A> 使用方法将用户定义的字符串插入到文本控件中。 否则， <xref:System.Windows.Forms.SendKeys.SendWait%2A?displayProperty=nameWithType> 使用方法。  
  
 [!code-csharp[InsertText#InsertText](../../../samples/snippets/csharp/VS_Snippets_Wpf/InsertText/CSharp/Window1.xaml.cs#inserttext)]
 [!code-vb[InsertText#InsertText](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InsertText/VisualBasic/Window1.xaml.vb#inserttext)]  
  
## <a name="see-also"></a>另请参阅

- [TextPattern 插入文本示例](/previous-versions/dotnet/netframework-3.5/ms771478(v=vs.90))
