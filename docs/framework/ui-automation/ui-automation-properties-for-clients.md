---
title: 客户端的 UI 自动化属性
description: 阅读有关 UI 自动化属性的概述，因为这些属性公开给 UI 自动化客户端应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties, UI Automation clients
- UI Automation, client properties
ms.assetid: 255905af-0b17-485c-93d4-8a2db2a6524b
ms.openlocfilehash: f86351f430e2d133a31cf6af81b41789ba444078
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279124"
---
# <a name="ui-automation-properties-for-clients"></a>客户端的 UI 自动化属性

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本概述为你介绍 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性，因为会向 UI 自动化客户端应用程序公开它们。  
  
 <xref:System.Windows.Automation.AutomationElement> 对象上的属性包含有关 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 元素（通常为控件）的信息。 <xref:System.Windows.Automation.AutomationElement> 的属性为泛型；即，不特定于控件类型。 其中的许多属性公开在 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation> 结构中。  
  
 控件模式也有属性。 控件模式的属性特定于该模式。 例如， <xref:System.Windows.Automation.ScrollPattern> 具有属性，该属性使客户端应用程序能够发现一个窗口是垂直可滚动还是水平可滚动，以及当前的视图大小和滚动位置。 控件模式通过一种结构公开其所有属性，例如， <xref:System.Windows.Automation.ScrollPattern.ScrollPatternInformation>。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性是只读的。 若要设置控件的属性，则必须使用相应的控件模式的方法。 例如，使用 <xref:System.Windows.Automation.ScrollPattern.Scroll%2A> 更改滚动窗口的位置值。  
  
 若要提高性能，则当检索 <xref:System.Windows.Automation.AutomationElement> 对象时可以缓存控件和控件模式的属性值。 有关详细信息，请参阅 [UI 自动化客户端中的缓存](caching-in-ui-automation-clients.md)。  
  
## <a name="property-ids"></a>属性 ID  

 Id)  (的属性标识符是在对象中封装的唯一常量值 <xref:System.Windows.Automation.AutomationProperty> 。 UI 自动化客户端应用程序从 <xref:System.Windows.Automation.AutomationElement> 类或从相应的控件模式类（如）获取这些 id <xref:System.Windows.Automation.ScrollPattern> 。 UI 自动化提供程序从 <xref:System.Windows.Automation.AutomationElementIdentifiers> 或从其中一个控件模式标识符类（如 <xref:System.Windows.Automation.ScrollPatternIdentifiers>）获取它们。  
  
 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> 的数值 <xref:System.Windows.Automation.AutomationProperty> 被提供程序用于标识正在 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPropertyValue%2A?displayProperty=nameWithType> 方法中进行查询的属性。 通常情况下，客户端应用程序不需要检查 <xref:System.Windows.Automation.AutomationIdentifier.Id%2A>。 <xref:System.Windows.Automation.AutomationIdentifier.ProgrammaticName%2A> 仅用于调试和诊断目的。  
  
## <a name="property-conditions"></a>属性条件  

 属性 Id 用于构造 <xref:System.Windows.Automation.PropertyCondition> 用于查找对象的对象 <xref:System.Windows.Automation.AutomationElement> 。 例如，你可能希望查找具有特定名称的 <xref:System.Windows.Automation.AutomationElement> 或已启用的所有控件。 每个 <xref:System.Windows.Automation.PropertyCondition> 指定 <xref:System.Windows.Automation.AutomationProperty> 标识符和属性必须匹配的值。  
  
 有关详细信息，请参阅以下参考主题：  
  
- <xref:System.Windows.Automation.AutomationElement.FindFirst%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.FindAll%2A>  
  
- <xref:System.Windows.Automation.TreeWalker.Condition%2A>  
  
## <a name="retrieving-properties"></a>检索属性  

 <xref:System.Windows.Automation.AutomationElement> 的某些属性和控件模式类的所有属性都公开为 `Current` 的嵌套属性或 `Cached` 的 <xref:System.Windows.Automation.AutomationElement> 属性或控件模式对象。  
  
 此外，任何 <xref:System.Windows.Automation.AutomationElement> 或控件模式属性，包括在 <xref:System.Windows.Automation.AutomationElement.Cached%2A> 或 <xref:System.Windows.Automation.AutomationElement.Current%2A> 结构中不可用的属性，都可以通过使用以下方法之一进行检索：  
  
- <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A>  
  
- <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A>  
  
 这些方法提供略好的性能以及对属性的完整范围的访问权限。  
  
 下面的代码示例演示在 <xref:System.Windows.Automation.AutomationElement>上检索属性的两种方法。  
  
 [!code-csharp[UIAClient_snip#121](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#121)]
 [!code-vb[UIAClient_snip#121](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#121)]  
  
 若要检索 <xref:System.Windows.Automation.AutomationElement>支持的控件模式的属性，则不需要检索控件模式对象。 只需将其中一个模式属性标识符传递给方法。  
  
 下面的代码示例演示在控件模式上检索属性的两种方法。  
  
 [!code-csharp[UIAClient_snip#122](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#122)]
 [!code-vb[UIAClient_snip#122](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#122)]  
  
 `Get` 方法返回 <xref:System.Object>。 应用程序必须在使用值之前将返回的对象转换为正确的类型。  
  
## <a name="default-property-values"></a>默认属性值  

 如果 UI 自动化提供程序不实现属性，则 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 系统能够提供一个默认值。 例如，如果控件的提供程序不支持由 <xref:System.Windows.Automation.AutomationElement.HelpTextProperty>标识的属性，则 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 返回一个空字符串。 同样，如果提供程序不支持由 <xref:System.Windows.Automation.AutomationElement.IsDockPatternAvailableProperty>标识的属性，则 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 返回 `false`。  
  
 你可以通过使用 <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> 和 <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> 方法重载更改此行为。 当指定 `true` 作为第二个参数时， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 不返回默认值，而是返回特殊值 <xref:System.Windows.Automation.AutomationElement.NotSupported>。  
  
 下面的示例代码尝试从某个元素检索属性，如果该属性不受支持，将会改为使用应用程序定义的值。  
  
 [!code-csharp[UIAClient_snip#123](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#123)]
 [!code-vb[UIAClient_snip#123](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#123)]  
  
 若要发现哪些属性受元素支持，请使用 <xref:System.Windows.Automation.AutomationElement.GetSupportedProperties%2A>。 这将返回 <xref:System.Windows.Automation.AutomationProperty> 标识符的数组。  
  
## <a name="property-changed-events"></a>属性更改事件  

 当 <xref:System.Windows.Automation.AutomationElement> 或控件模式上的属性值更改时，会引发一个事件。 应用程序可以通过调用 <xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A>订阅此类事件，提供 <xref:System.Windows.Automation.AutomationProperty> 标识符的数组作为最后的参数以便指定相关属性。  
  
 在 <xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>中，你可以通过检查事件参数的 <xref:System.Windows.Automation.AutomationPropertyChangedEventArgs.Property%2A> 成员来标识已更改的属性。 这些参数还包含已更改的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性的新旧值。 这些值属于类型 <xref:System.Object> ，并且必须在使用前转换为正确的类型。  
  
## <a name="additional-automationelement-properties"></a>其他 AutomationElement 属性  

 除 <xref:System.Windows.Automation.AutomationElement.Current%2A> 和 <xref:System.Windows.Automation.AutomationElement.Cached%2A> 属性结构以外， <xref:System.Windows.Automation.AutomationElement> 还具有以下属性，它们是通过简单的属性访问器检索的。  
  
|属性|描述|  
|--------------|-----------------|  
|<xref:System.Windows.Automation.AutomationElement.CachedChildren%2A>|缓存中的子 <xref:System.Windows.Automation.AutomationElement> 对象的集合。|  
|<xref:System.Windows.Automation.AutomationElement.CachedParent%2A>|缓存中的 <xref:System.Windows.Automation.AutomationElement> 父对象。|  
|<xref:System.Windows.Automation.AutomationElement.FocusedElement%2A>|（静态属性）具有输入焦点的 <xref:System.Windows.Automation.AutomationElement> 。|  
|<xref:System.Windows.Automation.AutomationElement.RootElement%2A>|（静态属性）根 <xref:System.Windows.Automation.AutomationElement>。|  
  
## <a name="see-also"></a>另请参阅

- [在 UI 自动化客户端中缓存](caching-in-ui-automation-clients.md)
- [服务器端 UI 自动化提供程序的实现](server-side-ui-automation-provider-implementation.md)
- [订阅 UI 自动化事件](subscribe-to-ui-automation-events.md)
