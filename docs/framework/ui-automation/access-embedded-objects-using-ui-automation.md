---
title: 使用 UI 自动化访问嵌入式对象
description: 请参阅如何在文本控件内容中使用 UI 自动化访问嵌入对象。 嵌入对象被视为 UI 自动化文本提供程序的子项。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- embedded objects, accessing
- accessing embedded objects
- UI Automation, accessing embedded objects
ms.assetid: a5b513ec-7fa6-4460-869f-c18ff04f7cf2
ms.openlocfilehash: 30b41e3a3d47802eb4a3e761c4282b3e937156f2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235774"
---
# <a name="access-embedded-objects-using-ui-automation"></a>使用 UI 自动化访问嵌入式对象

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题显示可如何使用 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] 来公开嵌入在文本控件内容中的对象。  
  
> [!NOTE]
> 嵌入对象可以包括图像、超链接、按钮、表或 ActiveX 控件。  
  
 嵌入对象可视为 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 文本提供程序的子项。 将能够通过与所有其他 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 元素相同的 UI 自动化树结构来公开嵌入对象。 而功能是通过嵌入对象控件类型通常需要的控件模式公开的（例如，由于超链接基于文本，因此超链接将支持 <xref:System.Windows.Automation.TextPattern>）。  
  
 ![文本容器中的嵌入式对象。](./media/uia-textpattern-embeddedobjects.PNG "UIA_TextPattern_EmbeddedObjects")  
带有文本内容的示例文档， ( "你知道吗？"... ) 和两个嵌入对象 (鲸鱼的图片和文本超链接) ，用作代码示例的目标。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何从 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 文本提供程序内检索嵌入对象的集合。 对于简介中提供的示例文档，将返回两个对象（一个图像元素和一个文本元素）。  
  
> [!NOTE]
> 图像元素应具有一些与图像关联的内部文本来描述图像，这些文本通常位于图像的 <xref:System.Windows.Automation.AutomationElement.NameProperty> 中（例如，“蓝鲸”）。 但是，如果获得了跨越图像对象的文本范围，文本流中既不会返回图像，也不会返回此描述性文本。  
  
[!code-csharp[FindText#StartApp](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#startapp)]
[!code-vb[FindText#StartApp](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#startapp)]  
[!code-csharp[FindText#FindTextProvider](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#findtextprovider)]
[!code-vb[FindText#FindTextProvider](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#findtextprovider)]  
[!code-csharp[FindText#GetChildren](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#getchildren)]
[!code-vb[FindText#GetChildren](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#getchildren)]  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何从 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 文本提供程序内的嵌入对象中获取文本范围。 检索的文本范围是一个空范围， 其中起始端点紧跟在“… ocean.”后的空格之后，结束端点位于表示嵌入超链接的结束“.”之前（如简介中提供的图像所示）。 即使这是一个空范围，也不会被视作退化的范围，因为它有非零跨度。  
  
> [!NOTE]
> <xref:System.Windows.Automation.TextPattern> 可以检索基于文本的嵌入对象（如超链接）；但是，将需要从嵌入对象中获取第二个 <xref:System.Windows.Automation.TextPattern> 才能公开其完整功能。  
  
 [!code-csharp[UIATextPattern_snip#GetRangeFromChild](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#getrangefromchild)]
 [!code-vb[UIATextPattern_snip#GetRangeFromChild](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIATextPattern_snip/VisualBasic/SearchWindow.vb#getrangefromchild)]  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化 TextPattern 概述](ui-automation-textpattern-overview.md)
- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [使用 UI 自动化向文本框添加内容](add-content-to-a-text-box-using-ui-automation.md)
- [使用 UI 自动化查找和突出显示文本](find-and-highlight-text-using-ui-automation.md)
