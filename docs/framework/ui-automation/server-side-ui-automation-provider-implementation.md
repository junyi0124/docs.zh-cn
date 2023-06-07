---
title: 服务器端 UI 自动化提供程序的实现
description: 了解如何在 .NET 中实现自定义控件的服务器端 UI 自动化提供程序。 WPF 和非 WPF 元素的实现不同。
ms.date: 03/30/2017
helpviewer_keywords:
- server-side UI Automation provider implementation
- UI Automation, server-side provider implementation
- provider implementation, UI Automation
ms.assetid: 6acc6d08-bd67-4e2e-915c-9c1d34eb86fe
ms.openlocfilehash: ee9fe5b3180abcc9ecbc4515e0af1e1c4b2e8b87
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555427"
---
# <a name="server-side-ui-automation-provider-implementation"></a>服务器端 UI 自动化提供程序的实现

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。

本部分将介绍如何实现自定义控件的服务器端 UI 自动化提供程序。

Windows Presentation Foundation (WPF) 元素和非 WPF Windows 窗体 (元素的实现在本质上是不同的。 WPF 元素通过从派生的类提供对的支持 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] <xref:System.Windows.Automation.Peers.AutomationPeer> 。 非 WPF 元素通过提供程序接口的实现提供支持。

<a name="Security_Considerations"></a>

## <a name="security-considerations"></a>安全注意事项

应编写提供程序，使它们能够在部分信任环境中的工作。 因为 UIAutomationClient.dll 未配置为在部分信任下运行，所以提供程序代码不应引用该程序集。 如果情况如此，则代码可以在完全信任环境中运行，但无法在部分信任环境中运行。

特别是，不要使用 UIAutomationClient.dll 中类的字段，例如 <xref:System.Windows.Automation.AutomationElement>中的字段。 请改用 UIAutomationTypes.dll 中类的等效字段，如 <xref:System.Windows.Automation.AutomationElementIdentifiers>。

<a name="Provider_Implementation_by_WPF_Elements"></a>

## <a name="provider-implementation-by-windows-presentation-foundation-elements"></a>通过 Windows Presentation Foundation 元素实现的提供程序实现

有关本主题的详细信息，请参阅 [WPF 自定义控件的 UI 自动化](/dotnet/desktop/wpf/controls/ui-automation-of-a-wpf-custom-control)。

<a name="Provider_Implementation_by_non_WPF_Elements"></a>

## <a name="provider-implementation-by-non-wpf-elements"></a>通过非 WPF 元素实现的提供程序实现

不属于 WPF 框架但以托管代码编写的自定义控件 (最常见的是 Windows 窗体控件) ， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 通过实现接口为提供支持。 每个元素必须实现至少一个下一部分中第一个表中列出的接口。 此外，如果该元素支持一个或多个控件模式，它必须实现每个控件模式的相应接口。

你的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序项目必须引用以下程序集：

- UIAutomationProviders.dll

- UIAutomationTypes.dll

- WindowsBase.dll

<a name="Provider_Interfaces"></a>

### <a name="provider-interfaces"></a>提供程序接口

每个 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序必须实现以下的一个接口。

|接口|说明|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderSimple>|为承载在窗口中的简单控件提供功能，包括支持控件模式和属性。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragment>|继承自 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple>。 为复杂控件中的元素添加功能，包括在片段中导航、设置焦点并返回该元素的边框。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>|继承自 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment>。 为复杂控件中的根元素添加功能，包括找到指定坐标处的子元素和设置整个控件的焦点状态。|

以下接口可提供额外的功能，但并不需要实现。

|接口|说明|
|---------------|-----------------|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|启用提供程序跟踪事件请求。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride>|启用对片段的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树内的基于窗口的元素的重定位。|

<xref:System.Windows.Automation.Provider> 命名空间中的所有其他接口用于控件模式支持。

<a name="Requirements_for_Non_WPF_Providers"></a>

### <a name="requirements-for-non-wpf-providers"></a>非 WPF 提供程序的要求

为了与 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]进行通信，你的控件必须实现以下主要领域的功能：

|功能|实现|
|-------------------|--------------------|
|向 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]公开提供程序|为响应发送到控制窗口的 WM_GETOBJECT 消息，返回实现 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple> （或派生接口）的对象。 对于片段，这必须是片段根的提供程序。|
|提供属性值|实现 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPropertyValue%2A> 以提供或重写值。|
|启用客户端从而与控件交互|实现支持控件模式的接口，如 <xref:System.Windows.Automation.Provider.IInvokeProvider>。 在你的 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.GetPatternProvider%2A>实现中返回这些模式提供程序。|
|引发事件|调用 <xref:System.Windows.Automation.Provider.AutomationInteropProvider> 的静态方法之一，以引发客户端可以侦听的一个事件。|
|启用导航并将在片段中设置焦点|为片段中的每个元素实现 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment> 。 （对于并非为片段的一部分的元素，此操作并不必要。）|
|启用设置焦点，以及查找片段中的子元素|实现 <xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot>。 （对于并非片段根的元素，此操作并不必要。）|

<a name="Property_Values_in_Non_WPF_Providers"></a>

### <a name="property-values-in-non-wpf-providers"></a>非 WPF 提供程序中的属性值

自定义控件的[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序必须支持特定属性，这些属性可由自动化系统以及客户端应用程序使用。 对于窗口 (HWND) 中承载的元素， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 可以从默认窗口提供程序检索某些属性，但必须从自定义提供程序获取其他属性。

基于 HWND 控件的提供程序通常不需要提供以下属性（由字段值标识）：

- <xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ProcessIdProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.ClassNameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.HasKeyboardFocusProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.IsPasswordProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>

- <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty>

> [!NOTE]
> 承载在窗口的简单元素或片段根的 <xref:System.Windows.Automation.AutomationElementIdentifiers.RuntimeIdProperty> 是从窗口中获取的；但是，根下的片段元素（如列表框中的列表项）必须提供自己的标识符。 有关详细信息，请参阅 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.GetRuntimeId%2A>。
>
> <xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>应为 Windows 窗体控件中承载的提供程序返回。 在这种情况下，默认的窗口提供程序可能无法检索正确值。
>
> <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> 通常由宿主提供程序提供。 例如，如果自定义控件从 <xref:System.Windows.Forms.Control>派生，则名称从控件的 `Text` 属性派生。

有关示例代码，请参阅 [Return Properties from a UI Automation Provider](return-properties-from-a-ui-automation-provider.md)。

<a name="Events_in_Non_WPF_Providers"></a>

### <a name="events-in-non-wpf-providers"></a>非 WPF 提供程序中的事件

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序应引发事件以通知客户端应用程序有关 UI 状态的变化。 以下方法用于引发事件。

|方法|说明|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationEvent%2A>|引发各种事件，包括由控件模式触发的事件。|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseAutomationPropertyChangedEvent%2A>|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 属性更改时引发事件。|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.RaiseStructureChangedEvent%2A>|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的结构更改时引发事件；例如，通过删除或添加一个元素。|

事件的目的是通知客户端 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)]中发生的情况，该活动是否由 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 系统本身触发。 例如，由 <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent> 标识的事件应当在每次调用该控件时引发，或通过直接用户输入或通过客户端应用程序调用 <xref:System.Windows.Automation.InvokePattern.Invoke%2A>。

若要优化性能，提供程序可以有选择地引发事件，或者，如果没有注册任何接收事件的客户端应用程序，则不引发任何事件。 以下方法用于进行优化。

|方法|说明|
|------------|-----------------|
|<xref:System.Windows.Automation.Provider.AutomationInteropProvider.ClientsAreListening%2A>|此静态属性指定是否存在已订阅 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 事件的客户端应用程序。|
|<xref:System.Windows.Automation.Provider.IRawElementProviderAdviseEvents>|提供程序在片段根上对此接口的实现使其能够在当客户端在片段上注册和注销事件处理程序时接收到通知。|

<a name="Non_WPF_Provider_Navigation"></a>

### <a name="non-wpf-provider-navigation"></a>非 WPF 提供程序导航

简单控件的提供程序，例如窗口 (HWND) 中承载的自定义按钮，无需支持 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中的导航。 导航到元素和从元素导航由主机窗口的默认提供程序处理，此程序在 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>的实现中指定。 但是，在实现复杂自定义控件的提供程序时，必须支持片段及其子代的根节点之间的导航，以及同级节点之间的导航。

> [!NOTE]
> 与根不同，片段的元素必须从 `null` 返回 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>引用，因为它们没有直接承载在窗口中，并且没有默认的提供程序可以支持往返它们的导航。

片段的结构由你的 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A>的实现决定。 对于自每个片段的每个可能的方向，此方法返回该方向的元素的提供程序对象。 如果该方向没有任何元素，则方法将返回 `null` 引用。

片段根仅支持子元素的导航。 例如，方向是 <xref:System.Windows.Automation.Provider.NavigateDirection.FirstChild>时，列表框将返回列表中的第一项，当方向是 <xref:System.Windows.Automation.Provider.NavigateDirection.LastChild>时，将返回最后一项。 片段根不支持导航到父级或同级；这由宿主窗口提供程序进行处理。

不是根的片段元素必须支持导航到父级、所有同级及其子级。

<a name="Non_WPF_Provider_Reparenting"></a>

### <a name="non-wpf-provider-reparenting"></a>非 WPF 提供程序重新设置父级

弹出窗口实际是顶级窗口，所以默认情况下会作为桌面的子级显示在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中。 但是，在许多情况下，弹出式窗口在逻辑上是一些其他控件的子级。 例如，组合框的下拉列表在逻辑上是组合框的子级。 同样，菜单弹出窗口在逻辑上是菜单的子级。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持重新设置弹出窗口的父级，以便它们显示为关联控件的子级。

若要重新设置弹出窗口的父级：

1. 为弹出窗口的创建一个提供程序。 这要求预先知道弹出窗口的类。

2. 照常实现弹出窗口的所有属性和模式，就像它是独立的控件一样。

3. 实现 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A> 属性，以便其返回从 <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A>获取的值，其中该参数是弹出窗口的窗口句柄。

4. 实现弹出窗口和其父级的 <xref:System.Windows.Automation.Provider.IRawElementProviderFragment.Navigate%2A> ，以便正确处理从逻辑父级到逻辑子级以及同级子级之间的导航。

当 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 遇到弹出窗口，它可以识别导航正从默认进行重写，并会跳过作为桌面子级出现的弹出窗口。 而节点将只能通过片段到达。

重新设置父级不适合控件可以承载任何类窗口的情况。 例如，rebar 在其带区中可承载任何类型的 HWND。 为了处理这些情况， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 支持 HWND 重定位的替代形式，如下节中所述。

<a name="Non_WPF_Provider_Repositioning"></a>

### <a name="non-wpf-provider-repositioning"></a>非 WPF 提供程序重定位

[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 片段可能包含两个或多个元素，其中每个元素都包含在一个窗口 (HWND) 中。 因为每个 HWND 都有自己的默认提供程序，该提供程序将 HWND 视为作为容器的 HWND 的子级，默认情况下， [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树将在片段中将 HWND 显示为父窗口的子级。 在大多数情况下，这是所需的行为，但有时这可能会导致混淆，因为它不匹配 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)]的逻辑结构。

一个很好的示例则是 rebar 控件。 Rebar 控件包含带区，其中每个带区又可以包含如工具栏、编辑框或组合框等基于 HWND 的控件。 Rebar HWND 的默认窗口提供程序将带区控件 HWND 视为子级，而 rebar 提供程序将带区视为子级。 因为 HWND 提供程序和 rebar 提供程序是协同工作，并且合并其子级，所以带区和基于 HWND 的控件都将显示为 rebar 的子级。 但是，在逻辑上，只有带区应显示为 rebar 的子级，并且每个带区提供程序应与它所包含控件的默认 HWND 提供程序结合使用。

为此，rebar 的片段根提供程序公开表示带区的子级集。 每个带区包含可能会公开属性和模式的单个提供程序。 在其实现 <xref:System.Windows.Automation.Provider.IRawElementProviderSimple.HostRawElementProvider%2A>的过程中，带区提供程序将返回控件 HWND的默认窗口提供程序，它通过调用 <xref:System.Windows.Automation.Provider.AutomationInteropProvider.HostProviderFromHandle%2A>获取此提供程序，并传入控件的窗口句柄。 最后，rebar 的片段根提供程序将实现 <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride> 接口，并在其实现 <xref:System.Windows.Automation.Provider.IRawElementProviderHwndOverride.GetOverrideProviderForHwnd%2A> 的过程中返回包含在指定 HWND 中的控件的相应带区提供程序。

## <a name="see-also"></a>请参阅

- [UI 自动化提供程序概述](ui-automation-providers-overview.md)
- [公开服务器端 UI 自动化提供程序](expose-a-server-side-ui-automation-provider.md)
- [从 UI 自动化提供程序返回属性](return-properties-from-a-ui-automation-provider.md)
- [从 UI 自动化提供程序中引发事件](raise-events-from-a-ui-automation-provider.md)
- [在 UI 自动化片段提供程序中启用导航](enable-navigation-in-a-ui-automation-fragment-provider.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
