---
title: 在 UI 自动化中使用缓存
description: 请参阅如何在 UI 自动化中使用缓存。 查看用于激活缓存请求、缓存 AutomationElement 属性和获取缓存模式的步骤。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- caching, UI Automation
- UI Automation, caching
ms.assetid: ec722dff-6009-4279-b86a-e18d3fa94ebf
ms.openlocfilehash: f99fb724130c359a77c72db66dd9f837ef1a2219
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258603"
---
# <a name="use-caching-in-ui-automation"></a>在 UI 自动化中使用缓存

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本部分演示如何实现 <xref:System.Windows.Automation.AutomationElement> 属性和控件模式的缓存。  
  
### <a name="activate-a-cache-request"></a>激活缓存请求  
  
1. 创建 <xref:System.Windows.Automation.CacheRequest>。  
  
2. 使用 <xref:System.Windows.Automation.CacheRequest.Add%2A>指定要缓存的属性和模式。  
  
3. 通过设置 <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> 属性来指定缓存的范围。  
  
4. 通过设置 <xref:System.Windows.Automation.CacheRequest.TreeFilter%2A> 属性来指定子树视图。  
  
5. 如果想通过不检索对对象的完全引用来提高效率，请将 <xref:System.Windows.Automation.CacheRequest.AutomationElementMode%2A> 属性设置为 <xref:System.Windows.Automation.AutomationElementMode.None> 。 （这将导致无法从那些对象中检索当前值。）  
  
6. <xref:System.Windows.Automation.CacheRequest.Activate%2A> `using` `Using` 在 Microsoft Visual Basic .net) 的块 (中使用激活请求。  
  
 在获得 <xref:System.Windows.Automation.AutomationElement> 对象或订阅事件后，通过使用 <xref:System.Windows.Automation.CacheRequest.Pop%2A> （如果使用了 <xref:System.Windows.Automation.CacheRequest.Push%2A> ）或处理 <xref:System.Windows.Automation.CacheRequest.Activate%2A>创建的对象来禁用请求。  (<xref:System.Windows.Automation.CacheRequest.Activate%2A> 在 `using` `Using` Microsoft Visual Basic .net) 中的块 (使用。  
  
### <a name="cache-automationelement-properties"></a>缓存 AutomationElement 属性  
  
1. 当 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时，使用 <xref:System.Windows.Automation.AutomationElement> 或 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> 获取 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>对象，或者获取 <xref:System.Windows.Automation.AutomationElement> 并将其作为 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时注册的事件的源。 （也可以通过下面的方法创建缓存：将 <xref:System.Windows.Automation.CacheRequest> 传递给 GetUpdatedCache 或 <xref:System.Windows.Automation.TreeWalker> 方法之一。）  
  
2. 使用 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A> 或从 <xref:System.Windows.Automation.AutomationElement.Cached%2A> 的 <xref:System.Windows.Automation.AutomationElement>属性中检索属性。  
  
### <a name="obtain-cached-patterns-and-their-properties"></a>获取缓存模式及其属性  
  
1. 当 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时，使用 <xref:System.Windows.Automation.AutomationElement> 或 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> 获取 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>对象，或者获取 <xref:System.Windows.Automation.AutomationElement> 并将其作为 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时注册的事件的源。 （也可以通过下面的方法创建缓存：将 <xref:System.Windows.Automation.CacheRequest> 传递给 GetUpdatedCache 或 <xref:System.Windows.Automation.TreeWalker> 方法之一。）  
  
2. 使用 <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> 或 <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> 检索缓存的模式。  
  
3. 控件模式的 `Cached` 属性中检索属性值。  
  
## <a name="example"></a>示例  

 下面的代码示例使用 <xref:System.Windows.Automation.CacheRequest.Activate%2A> 激活 <xref:System.Windows.Automation.CacheRequest>，演示了缓存的各个方面。  
  
 [!code-csharp[UIAClient_snip#107](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#107)]
 [!code-vb[UIAClient_snip#107](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#107)]  
  
## <a name="example"></a>示例  

 下面的代码示例使用 <xref:System.Windows.Automation.CacheRequest.Push%2A> 激活 <xref:System.Windows.Automation.CacheRequest>，演示了缓存的各个方面。 除非希望嵌套缓存请求，否则最好是使用 <xref:System.Windows.Automation.CacheRequest.Activate%2A>。  
  
 [!code-csharp[UIAClient_snip#108](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#108)]
 [!code-vb[UIAClient_snip#108](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#108)]  
  
## <a name="see-also"></a>另请参阅

- [在 UI 自动化客户端中缓存](caching-in-ui-automation-clients.md)
