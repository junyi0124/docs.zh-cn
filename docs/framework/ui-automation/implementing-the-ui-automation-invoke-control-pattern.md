---
title: 实现 UI 自动化 Invoke 控件模式
description: 阅读准则和约定，以实现 UI 自动化中的 Invoke 控件模式。 请参阅 Iinvokeprovider 必需接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Invoke control pattern
- control patterns, Invoke
- Invoke control pattern
ms.assetid: e5b1e239-49f8-468e-bfec-1fba02ec9ac4
ms.openlocfilehash: b464b3ab5cd2b0789798f8b865b946c5eae017eb
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166171"
---
# <a name="implementing-the-ui-automation-invoke-control-pattern"></a>实现 UI 自动化 Invoke 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。

本主题介绍了实现 <xref:System.Windows.Automation.Provider.IInvokeProvider>的准则和约定，包括有关事件和属性的信息。 本主题的结尾列出了指向其他参考资料的链接。

<xref:System.Windows.Automation.InvokePattern> 控件模式用于支持激活时不维护状态而是启动或执行单个明确操作的控件。 维护状态的控件（如复选框和单选按钮）则是必须分别实现 <xref:System.Windows.Automation.Provider.IToggleProvider> 和 <xref:System.Windows.Automation.Provider.ISelectionItemProvider> 。 有关实现此 Invoke 控件模式的控件的示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。

<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定

在实现 Invoke 控件模式时，请注意以下准则和约定：

- 如果不通过另一个控件模式提供程序公开同一行为，则控件实现 <xref:System.Windows.Automation.Provider.IInvokeProvider> 。 例如，如果控件上的 <xref:System.Windows.Automation.InvokePattern.Invoke%2A> 方法与 <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> 方法或 <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> 方法执行同一操作，则控件不应实现 <xref:System.Windows.Automation.Provider.IInvokeProvider>。

- 通常通过单击或双击或按 ENTER、预定义的键盘快捷键或某种备用的击键组合来调用控件。

- 在已被激活的控件上引发<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent> （作为对执行关联操作的控件的响应）。 如果可能，应在控件完成操作后引发事件且在不阻止的情况下返回事件。 在以下情况中，应在服务 Invoke 请求之前引发调用的事件：

  - 不可能等至操作完成，或这一做法不实际。

  - 该操作需要用户交互。

  - 该操作很耗时并且会导致调用的客户端花费大量的时间进行阻止。

- 如果调用该控件会产生巨大的负面影响，应通过 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.HelpText%2A> 属性公开这些副作用。 例如，即使 <xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> 不与所选内容相关联， <xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> 也可能会导致另一个控件变为处于选定状态。

- 悬停（或鼠标悬停）效果通常不会构成调用的事件。 但是，执行基于悬停状态的操作（而不是导致视觉效果）的控件应支持 <xref:System.Windows.Automation.InvokePattern> 控件模式。

> [!NOTE]
> 如果该控件仅可作为与鼠标相关的副作用的结果被调用，则此实现被视为可访问性问题。

- 调用一个控件不同于选择一个项。 但是，具体取决于控件，调用控件可能导致项被选为副作用。 例如，调用 "我的文档" 文件夹中的 Microsoft Word 文档列表项将选择该项并打开该文档。

- 元素被调用时将立即从 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中消失。 从由事件回调提供的元素请求信息可能失败。 建议的解决方法是预取缓存的信息。

- 控件可实现多个控件模式。 例如，Microsoft Excel 工具栏上的 "填充颜色" 控件同时实现 <xref:System.Windows.Automation.InvokePattern> 和 <xref:System.Windows.Automation.ExpandCollapsePattern> 控件模式。 <xref:System.Windows.Automation.ExpandCollapsePattern> 公开菜单，而 <xref:System.Windows.Automation.InvokePattern> 用所选颜色填充活动选择项。

<a name="Required_Members_for_the_IValueProvider_Interface"></a>

## <a name="required-members-for-iinvokeprovider"></a>IInvokeProvider 必需的成员

实现 <xref:System.Windows.Automation.Provider.IInvokeProvider>需要以下属性和方法。

|必需的成员|成员类型|说明|
|----------------------|-----------------|-----------|
|<xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A>|method|<xref:System.Windows.Automation.Provider.IInvokeProvider.Invoke%2A> 是一个异步调用且必须立即返回而不阻塞。<br /><br /> 此行为对于被调用时直接或间接启动模式对话框的控件而言尤其重要。 引发该事件的任何 UI 自动化客户端将保持被阻止的状态，直到模式对话框关闭为止。|

<a name="Exceptions"></a>

## <a name="exceptions"></a>例外

提供程序必须引发以下异常。

|异常类型|条件|
|--------------------|---------------|
|<xref:System.Windows.Automation.ElementNotEnabledException>|如果未启用该控件。|

## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [使用 UI 自动化调用控件](invoke-a-control-using-ui-automation.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
