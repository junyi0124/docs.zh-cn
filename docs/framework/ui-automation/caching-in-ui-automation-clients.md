---
title: 在 UI 自动化客户端中缓存
description: 获取有关 .NET 中 UI 自动化客户端中的缓存的详细信息。 缓存定义为数据的预提取。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation caching in clients
- caching, UI Automation clients
ms.assetid: 94c15031-4975-43cc-bcd5-c9439ed21c9c
ms.openlocfilehash: 029e292e60cd7c66d55b9567385bdd0e53fdebd4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283518"
---
# <a name="caching-in-ui-automation-clients"></a>在 UI 自动化客户端中缓存

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性和控件模式的缓存。  
  
 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]中，缓存意味着预提取的数据。 然后，无需进一步的跨进程通信即可访问数据。 UI 自动化客户端应用程序通常使用缓存来批量检索属性和控件模式。 然后，根据需要从缓存中检索信息。 应用程序会定期更新缓存，通常使为了响应表明 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 中的内容发生更改的事件。  
  
 使用具有服务器端 UI 自动化提供程序的 Windows Presentation Foundation (WPF) 控件和自定义控件，缓存的优势最明显。 访问客户端提供程序（如 Win32 控件的默认提供程序）的优点更小。  
  
 当应用程序激活 <xref:System.Windows.Automation.CacheRequest> ，然后使用任何返回 <xref:System.Windows.Automation.AutomationElement>的方法或属性时，就会进行缓存；例如， <xref:System.Windows.Automation.AutomationElement.FindFirst%2A>和 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>。 <xref:System.Windows.Automation.TreeWalker> 类的方法例外；只有在将 <xref:System.Windows.Automation.CacheRequest> 指定为参数（例如， <xref:System.Windows.Automation.TreeWalker.GetFirstChild%28System.Windows.Automation.AutomationElement%2CSystem.Windows.Automation.CacheRequest%29?displayProperty=nameWithType>）时，才会进行缓存。  
  
 如果 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时订阅事件，也会进行缓存。 作为事件来源传递到事件处理程序的 <xref:System.Windows.Automation.AutomationElement> 包含由原始 <xref:System.Windows.Automation.CacheRequest>指定的缓存属性和模式。 在订阅事件后对 <xref:System.Windows.Automation.CacheRequest> 进行的任何更改都没有影响。  
  
 可以缓存元素的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性和控件模式。  
  
<a name="Options_for_Caching"></a>

## <a name="options-for-caching"></a>缓存选项  

 <xref:System.Windows.Automation.CacheRequest> 指定以下缓存选项。  
  
<a name="Properties_to_Cache"></a>

### <a name="properties-to-cache"></a>要缓存的属性  

 可以通过在激活请求之前为每个属性调用 <xref:System.Windows.Automation.CacheRequest.Add%28System.Windows.Automation.AutomationProperty%29> 来指定要缓存的属性。  
  
<a name="Control_Patterns_to_Cache"></a>

### <a name="control-patterns-to-cache"></a>要缓存的控件模式  

 可以通过在激活请求之前为每个模式调用 <xref:System.Windows.Automation.CacheRequest.Add%28System.Windows.Automation.AutomationPattern%29> 来指定要缓存的控件模式。 在缓存模式时，不会自动缓存模式的属性；必须通过使用 <xref:System.Windows.Automation.CacheRequest.Add%2A?displayProperty=nameWithType>指定想要缓存的属性。  
  
<a name="Scope_of_the_Caching"></a>

### <a name="scope-and-filtering-of-caching"></a>缓存的范围和筛选  

 可以通过在激活请求之前设置 <xref:System.Windows.Automation.CacheRequest.TreeScope%2A?displayProperty=nameWithType> 属性来指定想要缓存其属性和模式的元素。 范围与请求处于活动状态时检索的元素有关。 例如，如果仅设置 <xref:System.Windows.Automation.TreeScope.Children>，然后检索 <xref:System.Windows.Automation.AutomationElement>，则会缓存该元素的子项的属性和模式，但不会缓存该元素本身的属性和模式。 若要确保对检索的元素本身进行缓存，则必须在 <xref:System.Windows.Automation.TreeScope.Element> 属性中包括 <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> 。 不可将范围设置为 <xref:System.Windows.Automation.TreeScope.Parent> 或 <xref:System.Windows.Automation.TreeScope.Ancestors>。 但是，在缓存了子元素后，就可以缓存父元素；请参阅本主题中的“检索缓存的子项和父项”。  
  
 缓存的范围还会受 <xref:System.Windows.Automation.CacheRequest.TreeFilter%2A?displayProperty=nameWithType> 属性的影响。 默认情况下，只会对出现在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图中的元素执行缓存。 但是，可以更改此属性以将缓存应用于所有元素，或者只应用于出现在内容视图中的元素。  
  
<a name="Strength_of_the_Element_References"></a>

### <a name="strength-of-the-element-references"></a>元素引用的强度  

 在检索 <xref:System.Windows.Automation.AutomationElement>时，默认情况下可以访问该元素的所有属性和模式，包括那些未缓存的属性和模式。 但是，为了提高效率，可以通过将 <xref:System.Windows.Automation.CacheRequest.AutomationElementMode%2A> 的 <xref:System.Windows.Automation.CacheRequest> 属性设置为 <xref:System.Windows.Automation.AutomationElementMode.None>来指定元素引用只引用缓存的数据。 在这种情况下，你没有权限访问所检索元素的任何未缓存属性和模式。 这意味着你无法通过 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> 或者 `Current` 的 <xref:System.Windows.Automation.AutomationElement> 属性或任何控件模式访问任何属性；也无法通过使用 <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> 或 <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A>检索模式。 在缓存的模式中，可以调用检索数组属性的方法（如 <xref:System.Windows.Automation.SelectionPattern.SelectionPatternInformation.GetSelection%2A?displayProperty=nameWithType>），但不能调用对控件执行操作的任何方法（如 <xref:System.Windows.Automation.InvokePattern.Invoke%2A?displayProperty=nameWithType>）。  
  
 可能无需完全引用对象的应用程序的一个示例是屏幕阅读器，它将预提取窗口中的元素的 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> 和 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> 属性，但不需要 <xref:System.Windows.Automation.AutomationElement> 对象本身。  
  
<a name="Activating_the_CacheRequest"></a>

## <a name="activating-the-cacherequest"></a>激活 CacheRequest  

 仅当在 <xref:System.Windows.Automation.AutomationElement> 对于当前线程处于活动状态的情况下检索 <xref:System.Windows.Automation.CacheRequest> 对象时，才会执行缓存。 有两种方法可激活 <xref:System.Windows.Automation.CacheRequest>。  
  
 通常的方法是调用 <xref:System.Windows.Automation.CacheRequest.Activate%2A>。 此方法返回实现 <xref:System.IDisposable>的对象。 只要 <xref:System.IDisposable> 对象存在，请求就将保持活动状态。 控制对象的生存期的最简单方法是将调用括在 `using` (c # 中 ) 或 `Using` (Visual Basic) 块。 这样即使引发异常也能确保请求从堆栈中弹出。  
  
 另一种方法是调用 <xref:System.Windows.Automation.CacheRequest.Push%2A>，此方法在你想要嵌套缓存请求时非常有用。 此方法将请求放在堆栈上，并激活请求。 在 <xref:System.Windows.Automation.CacheRequest.Pop%2A>从堆栈中删除请求之前，请求将保持活动状态。 如果将另一个请求推入堆栈，该请求将暂时变为非活动状态；只有堆栈顶部的请求才会处于活动状态。  
  
<a name="Retrieving_Cached_Properties"></a>

## <a name="retrieving-cached-properties"></a>检索缓存的属性  

 你可以通过以下方法和属性检索元素的缓存属性。  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.Cached%2A>  
  
 如果请求的属性不在缓存中，则会引发异常。  
  
 <xref:System.Windows.Automation.AutomationElement.Cached%2A>像 <xref:System.Windows.Automation.AutomationElement.Current%2A>，都以结构成员的方式公开各项属性。 但是，无需检索此结构；你可以直接访问各项属性。 例如，可以从 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> 中获取 `element.Cached.Name`属性，其中 `element` 为 <xref:System.Windows.Automation.AutomationElement>。  
  
<a name="Retrieving_Cached_Control_Patterns"></a>

## <a name="retrieving-cached-control-patterns"></a>检索缓存的控件模式  

 可以通过以下方法检索元素的缓存控件模式。  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A>  
  
 如果模式不在缓存中，则 <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A> 会引发异常，并且 <xref:System.Windows.Automation.AutomationElement.TryGetCachedPattern%2A> 会返回 `false`。  
  
 可以通过使用模式对象的 `Cached` 属性来检索控件模式的缓存属性。 也可以通过 `Current` 属性检索当前值，但只有当检索 <xref:System.Windows.Automation.AutomationElementMode.None> 时未指定 <xref:System.Windows.Automation.AutomationElement> 的情况下才能这样做。 （默认值为<xref:System.Windows.Automation.AutomationElementMode.Full> ，并且此值允许访问当前值。）  
  
<a name="Retrieving_Cached_Children_and_Parents"></a>

## <a name="retrieving-cached-children-and-parents"></a>检索缓存的子项和父项  

 如果检索 <xref:System.Windows.Automation.AutomationElement> 并通过请求的 <xref:System.Windows.Automation.CacheRequest.TreeScope%2A> 属性请求对该元素的子项进行缓存，随后将可以从所检索元素的 <xref:System.Windows.Automation.AutomationElement.CachedChildren%2A> 属性中获取子元素。  
  
 如果 <xref:System.Windows.Automation.TreeScope.Element> 已包含在缓存请求的范围内，则随后可从任何子元素的 <xref:System.Windows.Automation.AutomationElement.CachedParent%2A> 属性中获取请求的根元素。  
  
> [!NOTE]
> 无法缓存请求根元素的父项或上级。  
  
<a name="Updating_the_Cache"></a>

## <a name="updating-the-cache"></a>更新缓存  

 只要 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]中没有更改，缓存就有效。 应用程序负责更新缓存，通常是为了响应事件。  
  
 如果在 <xref:System.Windows.Automation.CacheRequest> 处于活动状态时订阅事件，则每次调用事件处理程序委托时都会获得一个使用更新的缓存作为事件源的 <xref:System.Windows.Automation.AutomationElement> 。 还可以通过调用 <xref:System.Windows.Automation.AutomationElement.GetUpdatedCache%2A>来更新元素的缓存信息。 可以传入原始 <xref:System.Windows.Automation.CacheRequest> 来更新以前缓存的所有信息。  
  
 更新缓存不会更改任何现有 <xref:System.Windows.Automation.AutomationElement> 引用的属性。  
  
## <a name="see-also"></a>另请参阅

- [客户端的 UI 自动化事件](ui-automation-events-for-clients.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
- [FetchTimer 示例](/previous-versions/dotnet/netframework-3.5/ms771456(v=vs.90))
