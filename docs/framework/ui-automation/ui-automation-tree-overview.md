---
title: UI 自动化树概述
description: 阅读有关 UI 自动化树的概述。 了解 UI 自动化树的不同视图，例如原始视图、控件视图和内容视图。
ms.date: 03/30/2017
helpviewer_keywords:
- automation tree
- UI Automation, tree
ms.assetid: 03b98058-bdb3-47a0-8ff7-45e6cdf67166
ms.openlocfilehash: 99735007a3058f45585e812e2ec3f955d7a2c30d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258655"
---
# <a name="ui-automation-tree-overview"></a>UI 自动化树概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 辅助技术产品和测试脚本通过在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中导航来收集有关 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 及其元素的信息。  
  
 在 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树中，有一个根元素 (<xref:System.Windows.Automation.AutomationElement.RootElement%2A> 表示当前桌面的) ，其子元素表示应用程序窗口。 其中的每个子元素都可以包含表示 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 的各个部分（如菜单、按钮、工具栏和列表框）的元素。 这些子元素又可以包含列表项之类的元素。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的结构不固定，由于它可能包含数千个元素，因此它很少全部显示出来。 可以根据需要生成树的某些部分，添加、移动或删除元素时，树也会发生相应更改。  
  
 UI 自动化提供程序通过实现片段（由一个通常承载在窗口中的根元素和一个子树组成）中各项之间的导航来支持 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树。 但是，提供程序并不参与控件之间的导航。 控件之间的导航是由 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 核心借助于默认窗口提供程序中的信息来管理的。  
  
<a name="uiautomation_tree_view"></a>

## <a name="views-of-the-automation-tree"></a>自动化树的视图  

 可以对 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树进行筛选，以便创建仅包含与特定客户端相关的 <xref:System.Windows.Automation.AutomationElement> 对象的视图。 此方法允许客户端根据各自的具体需求来自定义通过 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 表示的结构。  
  
 客户端可以通过以下两种方法来自定义视图：设置范围和筛选。 设置范围是指从基元素开始定义视图的范围，例如，应用程序可能希望仅查找桌面的直接子级，或者查找某个应用程序窗口的所有后代。 筛选是指定义要包括在视图中的元素类型。  
  
 UI 自动化提供程序通过定义元素的属性（包括 <xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty> 和 <xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty> 属性）来支持筛选。  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供三个默认视图。 这些视图由所执行筛选的类型定义；任何视图的范围都是由应用程序定义的。 另外，应用程序可以向属性应用其他筛选器，例如，在控件视图中仅包含已启用的控件。  
  
<a name="uiautomation_raw_view"></a>

### <a name="raw-view"></a>原始视图  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的原始视图是 <xref:System.Windows.Automation.AutomationElement> 对象的完整树，该树的根元素是桌面。 原始视图完全遵循应用程序的本机编程结构，因此是最详细的可用视图。 原始视图还是其他树视图的生成基础。 由于此视图依赖于基础 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 框架，因此按钮的原始视图 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 将与 Win32 按钮具有不同的原始视图。  
  
 原始视图是通过以下方法来获取的：在不指定属性的情况下搜索元素，或者使用 <xref:System.Windows.Automation.TreeWalker.RawViewWalker> 在树中导航。  
  
<a name="uiautomation_control_view"></a>

### <a name="control-view"></a>控件视图  

 由于 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的控件视图紧密映射到由最终用户查看的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 结构，因此它能使辅助技术产品更轻松地完成向最终用户描述 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 并帮助最终用户与应用程序交互的任务。  
  
 控件视图是原始视图的子集。 它包括原始视图中的所有 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项，最终用户会将这些项理解为 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 中控件逻辑结构的交互式项或构成项。 例如，列表视图标题、工具栏、菜单和状态栏等项容器构成了 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 的逻辑结构，但其本身并不是交互式 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项。 仅针对布局或修饰目的而使用的非交互式项将不会显示在控件视图中。 仅为了将控件放置在对话框中而本身不包含任何信息的面板就是这样的项。 对话框中包含信息和静态文本的图形是在控件视图中显示的非交互式项。 控件视图中包括的非交互式项不能接收键盘焦点。  
  
 控件视图是通过以下方法获取的：搜索 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> 属性设置为 `true` 的元素，或者使用 <xref:System.Windows.Automation.TreeWalker.ControlViewWalker> 在树中导航。  
  
<a name="uiautomation_content_view"></a>

### <a name="content-view"></a>内容视图  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树的内容视图是控件视图的子集。 内容视图中包含用来在用户界面中传达真正信息的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项（包括可以接收键盘焦点的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项，以及一些不是 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项标签的文本）。 例如，下拉组合框中的值将出现在内容视图中，因为它们表示正由最终用户使用的信息。 在内容视图中，组合框和列表框均表示为 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 项的集合，在该集合中，可以有一项（或多项）处于选中状态。 由于内容视图旨在显示要呈现给用户的数据或内容，因此，内容视图中不存在如下情况：始终有一项处于打开状态，而另有一项是可以展开和折叠的。  
  
 内容视图是通过以下方法获取的：搜索 <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> 属性设置为 `true` 的元素，或者使用 <xref:System.Windows.Automation.TreeWalker.ContentViewWalker> 在树中导航。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Windows.Automation.AutomationElement>
- [UI 自动化概述](ui-automation-overview.md)
