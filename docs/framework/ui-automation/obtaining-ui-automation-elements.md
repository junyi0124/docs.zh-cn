---
title: 获取 UI 自动化元素
description: 查看用于获取 UI 自动化元素 () 用户界面 (UI) 元素的各种方法。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, obtaining elements
- elements, UI Automation, obtaining
ms.assetid: c2caaf45-e59c-42a1-bc9b-77a6de520171
ms.openlocfilehash: 66a8d9e944a8f8a777ec744a33f2cdb2d33e200c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237347"
---
# <a name="obtaining-ui-automation-elements"></a>获取 UI 自动化元素

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题描述获取 <xref:System.Windows.Automation.AutomationElement> 元素的 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 对象的各种方法。  
  
> [!CAUTION]
> 如果客户端应用程序可以尝试在其自己的用户界面中查找元素，则必须在一个单独的线程上进行所有 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 调用。 有关更多信息，请参见 [UI Automation Threading Issues](ui-automation-threading-issues.md)。  
  
<a name="The_Root_Element"></a>

## <a name="root-element"></a>Root 元素  

 所有 <xref:System.Windows.Automation.AutomationElement> 对象搜索都必须具有一个起点。 此起点可以是任何元素，包括桌面、应用程序窗口或控件。  
  
 所有元素所起源于的桌面根元素是从静态的 <xref:System.Windows.Automation.AutomationElement.RootElement%2A?displayProperty=nameWithType> 属性中获取的。  
  
> [!CAUTION]
> 通常，你应当尝试只获取 <xref:System.Windows.Automation.AutomationElement.RootElement%2A>的直接子项。 对后代的搜索可能循环访问数百个甚至数千个元素，这可能导致堆栈溢出。 如果尝试在较低级别上获取特定元素，应该从应用程序窗口或者从较低级别的容器中开始搜索。  
  
<a name="Using_Conditions"></a>

## <a name="conditions"></a>Conditions  

 对于用于检索 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素的大多数方法，必须指定一个 <xref:System.Windows.Automation.Condition>，这是一组用于定义要检索的元素的条件。  
  
 最简单的条件是 <xref:System.Windows.Automation.Condition.TrueCondition>，这是一个指定要返回搜索范围内的所有元素的预定义对象。 <xref:System.Windows.Automation.Condition.FalseCondition>是 <xref:System.Windows.Automation.Condition.TrueCondition>的对立条件，其作用不大，因为它将阻止找到任何元素。  
  
 下面是三个其他的预定义条件，这些条件既可以单独使用，也可以与其他条件一起使用： <xref:System.Windows.Automation.Automation.ContentViewCondition>、 <xref:System.Windows.Automation.Automation.ControlViewCondition>和 <xref:System.Windows.Automation.Automation.RawViewCondition>。 单独使用的等效于 ，因为它不根据元素的  或  属性来筛选元素。  
  
 其他条件是根据一个或多个 <xref:System.Windows.Automation.PropertyCondition> 对象（每个对象都指定一个属性值）建立的。 例如，个 <xref:System.Windows.Automation.PropertyCondition> 可以指定元素处于启用状态或元素支持某种控件模式。  
  
 通过构造 <xref:System.Windows.Automation.AndCondition>、 <xref:System.Windows.Automation.OrCondition>和 <xref:System.Windows.Automation.NotCondition>类型的对象，可以用布尔逻辑将条件结合起来。  
  
<a name="Search_Scope"></a>

## <a name="search-scope"></a>搜索范围  

 用 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> 或 <xref:System.Windows.Automation.AutomationElement.FindAll%2A> 执行的搜索必须具有一个范围和一个起点。  
  
 范围定义了要搜索的起点周围的空间。 这可以包括元素本身、其同级项、父项、上级、直接子项以及后代。  
  
 搜索范围由按位组合的 <xref:System.Windows.Automation.TreeScope> 枚举值来定义。  
  
<a name="Finding_a_Known_Element"></a>

## <a name="finding-a-known-element"></a>查找已知元素  

 若要查找由已知元素的 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A>、 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AutomationId%2A>或者某些其他属性或属性组合标识的已知元素，最简单的方法是使用 <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> 方法。 如果查找的元素是一个应用程序窗口，则搜索的起点可以是 <xref:System.Windows.Automation.AutomationElement.RootElement%2A>。  
  
 这种查找 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素的方法在自动化测试方案中最有用。  
  
<a name="Finding_Elements_in_a_Subtree"></a>

## <a name="finding-elements-in-a-subtree"></a>查找子树中的元素  

 若要查找与已知元素有关且满足特定条件的所有元素，可以使用 <xref:System.Windows.Automation.AutomationElement.FindAll%2A>。 例如，可以使用这种方法从列表或菜单中检索列表项或菜单项，或找出对话框中的所有控件。  
  
<a name="Walking_a_Subtree"></a>

## <a name="walking-a-subtree"></a>浏览子树  

 如果你事先不了解可与客户端一起使用的应用程序，则可以使用 <xref:System.Windows.Automation.TreeWalker> 类构造一个包含所有相关元素的子树。 你的应用程序可能会执行此操作以响应焦点更改事件；也就是说，在应用程序或控件接收输入焦点时，UI 自动化客户端将检查子项，可能还会检查有焦点的元素的所有后代。  
  
 使用 <xref:System.Windows.Automation.TreeWalker> 的另一种方法是识别元素的上级。 例如，通过向上浏览树，你可以识别控件的父窗口。  
  
 通过创建类的对象（通过传递 <xref:System.Windows.Automation.TreeWalker> 来定义相关元素），或者使用以下定义为 <xref:System.Windows.Automation.Condition>的字段的预定义对象之一，你可以使用 <xref:System.Windows.Automation.TreeWalker>。  
  
|||  
|-|-|  
|<xref:System.Windows.Automation.TreeWalker.ContentViewWalker>|仅查找 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> 属性为 `true`的元素。|  
|<xref:System.Windows.Automation.TreeWalker.ControlViewWalker>|仅查找 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> 属性为 `true`的元素。|  
|<xref:System.Windows.Automation.TreeWalker.RawViewWalker>|查找所有元素。|  
  
 在获得 <xref:System.Windows.Automation.TreeWalker>之后，可以直接使用它。 只需调用 `Get` 方法便可在子树的元素中导航。  
  
 <xref:System.Windows.Automation.TreeWalker.Normalize%2A> 方法可用于从视图外的另一元素导航到子树中的某个元素。 例如，假设你已使用 <xref:System.Windows.Automation.TreeWalker.ContentViewWalker>创建了一个子树视图。 然后，你的应用程序收到通知，得知滚动条已经接收了输入焦点。 因为滚动条不是内容元素，所以子树视图中未呈现滚动条。 但是，你可以将表示滚动条的 <xref:System.Windows.Automation.AutomationElement> 传递到 <xref:System.Windows.Automation.TreeWalker.Normalize%2A> 并检索内容视图中最接近的上级。  
  
<a name="Other_Ways_to_Retrieve_an_Element"></a>

## <a name="other-ways-to-retrieve-an-element"></a>检索元素的其他方法  

 除了搜索和导航，你还可以通过以下方法来检索 <xref:System.Windows.Automation.AutomationElement> 。  
  
### <a name="from-an-event"></a>从事件中  

 当你的应用程序接收 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件时，传递到事件处理程序的源对象是 <xref:System.Windows.Automation.AutomationElement>。 例如，如果你订阅焦点更改事件，则传递到 <xref:System.Windows.Automation.AutomationFocusChangedEventHandler> 的源是收到焦点的元素。  
  
 有关详细信息，请参阅 [Subscribe to UI Automation Events](subscribe-to-ui-automation-events.md)。  
  
### <a name="from-a-point"></a>从某个点中  

 如果你具有屏幕坐标（例如，光标位置），则可以使用静态 <xref:System.Windows.Automation.AutomationElement> 方法来检索 <xref:System.Windows.Automation.AutomationElement.FromPoint%2A> 。  
  
### <a name="from-a-window-handle"></a>从窗口句柄中  

 若要从 HWND 中检索 <xref:System.Windows.Automation.AutomationElement> ，请使用静态 <xref:System.Windows.Automation.AutomationElement.FromHandle%2A> 方法。  
  
### <a name="from-the-focused-control"></a>从具有焦点的控件中  

 可以从静态 <xref:System.Windows.Automation.AutomationElement> 属性中检索表示焦点控件的 <xref:System.Windows.Automation.AutomationElement.FocusedElement%2A> 。  
  
## <a name="see-also"></a>另请参阅

- [基于属性条件查找 UI 自动化元素](find-a-ui-automation-element-based-on-a-property-condition.md)
- [使用 TreeWalker 在 UI 自动化元素之间导航](navigate-among-ui-automation-elements-with-treewalker.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
