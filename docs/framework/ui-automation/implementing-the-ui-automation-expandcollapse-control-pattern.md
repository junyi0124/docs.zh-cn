---
title: 实现 UI 自动化 ExpandCollapse 控件模式
description: 了解 UI 自动化中 ExpandCollapse 控件模式的实现准则和约定。 知道如何实现 Iexpandcollapseprovider 必需。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, ExpandCollapse control pattern
- ExpandCollapse control pattern
- control patterns, ExpandCollapse
ms.assetid: 1dbabb8c-0d68-47c1-a35e-1c01cb01af26
ms.openlocfilehash: 525b57816071ba2d879036676201a0506d1a29db
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165858"
---
# <a name="implementing-the-ui-automation-expandcollapse-control-pattern"></a>实现 UI 自动化 ExpandCollapse 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。

本主题介绍实现 <xref:System.Windows.Automation.Provider.IExpandCollapseProvider>的准则和约定，包括有关属性、方法和事件的信息。 本概述的结尾列出了指向其他参考资料的链接。

<xref:System.Windows.Automation.ExpandCollapsePattern> 控件模式用于支持以可视化方式展开来显示更多内容或者以可视化方式折叠来隐藏相关内容的控件。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。

<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定

在实现 ExpandCollapse 控件模式时，请注意以下准则和约定：

- 聚合控件（通过一些子对象生成，这些子对象提供具有展开/折叠功能的 UI）必须支持 <xref:System.Windows.Automation.ExpandCollapsePattern> 控件模式，而它们的子元素不必支持。 例如，组合框控件是由列表框、按钮和编辑控件组合而成的，但它是唯一一个必须支持 <xref:System.Windows.Automation.ExpandCollapsePattern>的父组合框。

  > [!NOTE]
  > 菜单控件是一个例外，它是对各个 MenuItem 对象的聚合。 MenuItem 对象可以支持 <xref:System.Windows.Automation.ExpandCollapsePattern> 控件模式，但是父 Menu 控件无法支持。 Tree 和 Tree Item 控件也存在类似的例外。

- 将某个控件的 <xref:System.Windows.Automation.ExpandCollapseState> 设置为 <xref:System.Windows.Automation.ExpandCollapseState.LeafNode>后，该控件的所有 <xref:System.Windows.Automation.ExpandCollapsePattern> 功能当前都处于非活动状态，通过该控件模式可以获取的唯一信息就是 <xref:System.Windows.Automation.ExpandCollapseState>。 如果以后添加任何子对象，则 <xref:System.Windows.Automation.ExpandCollapseState> 会发生更改，同时 <xref:System.Windows.Automation.ExpandCollapsePattern> 功能将被激活。

- <xref:System.Windows.Automation.ExpandCollapseState> 仅表示直接子对象的可见性，而不表示所有后代对象的可见性。

- 展开和折叠功能是特定于控件的。 下面是该行为的示例。

  - Office Personal Menu 可以是具有三种状态（<xref:System.Windows.Automation.ExpandCollapseState.Expanded>、 <xref:System.Windows.Automation.ExpandCollapseState.Collapsed> 和 <xref:System.Windows.Automation.ExpandCollapseState.PartiallyExpanded>）的 MenuItem，该 MenuItem 中的控件指定在调用 <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> 或 <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> 时要采用的状态。

  - 对 TreeItem 调用 <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> 可能会显示所有后代，也可能仅显示直接子级。

  - 如果对某个控件调用 <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> 或 <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> 以保持该控件的后代的状态，则应当发送可见性更改事件，而不是状态更改事件。如果折叠父控件无法保持该控件的后代的状态，则该控件可能会销毁所有不再可见的后代并引发一个销毁事件，也可能会更改每个后代的 <xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A> 并引发一个可见性更改事件。

- 为保证导航操作，最好让对象保留在具有相应可见性状态的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中，而不管其父级的 <xref:System.Windows.Automation.ExpandCollapseState>如何。 如果后代是按需生成的，那么，仅当它们在首次显示之后或者可见时，才能出现在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中。

<a name="Required_Members_for_the_IValueProvider_Interface"></a>

## <a name="required-members-for-iexpandcollapseprovider"></a>IExpandCollapseProvider 必需的成员

实现 <xref:System.Windows.Automation.Provider.IExpandCollapseProvider>需要以下属性和方法。

|必需的成员|成员类型|说明|
|----------------------|-----------------|-----------|
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A>|Property|无|
|<xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A>|方法|无|
|<xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A>|方法|无|
|<xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>|事件|此控件没有关联的事件；请使用此泛型委托。|

<a name="Exceptions"></a>

## <a name="exceptions"></a>例外

提供程序必须引发以下异常。

|例外类型|条件|
|--------------------|---------------|
|<xref:System.InvalidOperationException>|当 <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> 或 <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> 时，将调用 <xref:System.Windows.Automation.ExpandCollapseState> = <xref:System.Windows.Automation.ExpandCollapseState.LeafNode>。|

## <a name="see-also"></a>请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [使用 TreeWalker 在 UI 自动化元素之间导航](navigate-among-ui-automation-elements-with-treewalker.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
