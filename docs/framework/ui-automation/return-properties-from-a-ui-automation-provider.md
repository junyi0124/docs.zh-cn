---
title: 从 UI 自动化提供程序返回属性
description: 了解 UI 自动化提供程序如何才能将元素的属性返回到 .NET 中的客户端应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- providers, UI Automation, returning properties
- properties, returned by UI Automation providers
- UI Automation, providers returning properties
ms.assetid: 5eba950e-b9e1-48eb-ab8e-b69db76bf589
ms.openlocfilehash: 8269d7d04a67c2a89a28160c0a8bc82bd627749f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250471"
---
# <a name="return-properties-from-a-ui-automation-provider"></a>从 UI 自动化提供程序返回属性

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题包含显示 UI 自动化提供程序如何才能将元素的属性返回到客户端应用程序的示例代码。  
  
 对于不显式支持的任何属性，则提供程序必须返回 `null` （Visual Basic 中为`Nothing` ）。 这样可确保 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 试图获取来自另一个源（如宿主窗口提供程序）的属性。  
  
## <a name="example"></a>示例  

 [!code-csharp[UIAFragmentProvider_snip#117](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFragmentProvider_snip/CSharp/ListFragment.cs#117)]
 [!code-vb[UIAFragmentProvider_snip#117](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFragmentProvider_snip/VisualBasic/ListFragment.vb#117)]  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化提供程序概述](ui-automation-providers-overview.md)
- [服务器端 UI 自动化提供程序的实现](server-side-ui-automation-provider-implementation.md)
