---
title: 在 UI 自动化片段提供程序中启用导航
description: 阅读一个示例，该示例演示如何在 UI 自动化提供程序中为片段中的元素启用导航。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, enabling navigation in provider
- navigation, enabling in UI Automation provider
ms.assetid: 3cb6092a-58c9-4ca0-84a5-0e54d5d00a0d
ms.openlocfilehash: d8fb67a84b7cba84fe65cd2f87baa6549122d2a2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276498"
---
# <a name="enable-navigation-in-a-ui-automation-fragment-provider"></a>在 UI 自动化片段提供程序中启用导航

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题包含示例代码，该示例代码演示如何为片段中的元素启用在 UI 自动化提供程序中导航的功能。  
  
## <a name="example"></a>示例  

 下面的示例代码为列表中的列表项实现 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> 父元素是列表框元素，且同级元素是列表集合中的其他项。 对于无效的方向，该方法返回 `null` （在 Visual Basic 中则返回`Nothing` ）；在本例中，将返回 <xref:System.Windows.Automation.Provider.NavigateDirection.FirstChild> 和 <xref:System.Windows.Automation.Provider.NavigateDirection.LastChild>，因为该元素没有任何子级。  
  
 [!code-csharp[UIAFragmentProvider_snip#103](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFragmentProvider_snip/CSharp/ListItemFragment.cs#103)]
 [!code-vb[UIAFragmentProvider_snip#103](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFragmentProvider_snip/VisualBasic/ListItemFragment.vb#103)]  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化提供程序概述](ui-automation-providers-overview.md)
- [服务器端 UI 自动化提供程序的实现](server-side-ui-automation-provider-implementation.md)
