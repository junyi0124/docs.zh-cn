---
title: UI 自动化提供程序概述
description: 请参阅 UI 自动化提供程序的概述，使控件能够与 UI 自动化客户端应用程序进行通信。 查看提供程序类型和提供程序的概念。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- providers, UI Automation
ms.assetid: 859557b8-51e1-4d15-92e8-318d2dcdb2f7
ms.openlocfilehash: 93a80cd35cc23fc0cdfb88620c310397d155b3d9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240363"
---
# <a name="ui-automation-providers-overview"></a>UI 自动化提供程序概述

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 UI 自动化提供程序使控件能够与 UI 自动化客户端应用程序进行通信。 通常， [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 中的每个控件或其他非重复元素均由提供程序表示。 提供程序公开有关该元素的信息并选择性实现控件模式，这些空间模式使客户端应用程序能够与控件进行交互。  
  
 客户端应用程序通常不需要直接使用提供程序。 使用 Win32、Windows 窗体或框架的应用程序中的大多数标准控件 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] 自动公开给 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 系统。 实现自定义控件的应用程序也可实现这些控件的 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序，客户端应用程序不需要采取任何特殊措施来获得其访问权限。  
  
 本主题概述控件开发人员如何实现 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 提供程序，特别是对于 Windows 窗体和 Win32 窗口中的控件。  
  
<a name="Types_of_Providers"></a>

## <a name="types-of-providers"></a>提供程序类型  

 UI 自动化提供程序分为两类：客户端提供程序和服务器端提供程序。  
  
### <a name="client-side-providers"></a>客户端提供程序  

 客户端提供程序由 UI 自动化客户端实现，以便与不支持（或不完全支持） [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的应用程序进行通信。 通常，客户端提供程序通过发送和接收 Windows 消息来跨进程边界与服务器通信。  
  
 由于 Win32、Windows 窗体或应用程序中控件的 UI 自动化提供程序 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 是作为操作系统的一部分提供的，因此客户端应用程序很少需要实现自己的提供程序，并且此概述不会进一步涵盖它们。  
  
### <a name="server-side-providers"></a>服务器端提供程序  

 服务器端提供程序由自定义控件或基于非 Win32、Windows 窗体或的 UI 框架的应用程序实现 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 。  
  
 服务器端提供程序向 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 核心系统公开接口，核心系统依次处理来自客户端的请求，使服务器端提供程序与跨进程边界的客户端应用程序进行通信。  
  
<a name="AutomationProviderConcepts"></a>

## <a name="ui-automation-provider-concepts"></a>UI 自动化提供程序概念  

 本部分简要介绍实现 UI 自动化提供程序所需了解的一些关键概念。  
  
### <a name="elements"></a>元素  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素是 [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] 的片段，对 UI 自动化客户端可见。 示例包括应用程序窗口、窗格、按钮、工具提示、列表框和列表项。  
  
### <a name="navigation"></a>导航  

 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素作为 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树公开给客户端。 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 通过从在元素之间进行导航来构造该树。 提供程序为每个元素启用导航，每个导航可能指向一个父级、同级和子级元素。  
  
 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树客户端视图的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
### <a name="views"></a>视图  

 客户端在三个主要视图中显示 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树，如下表所示。  
  
|||  
|-|-|  
|原始视图|包含所有元素。|  
|控件视图|包含控件元素。|  
|内容视图|包含具有内容的元素。|  
  
 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 树客户端视图的详细信息，请参阅 [UI Automation Tree Overview](ui-automation-tree-overview.md)。  
  
 提供程序实现负责将元素为定义为内容元素或控件元素。 控件元素可以是也可以不是内容元素，但所有内容元素都是控件元素。  
  
### <a name="frameworks"></a>框架  

 框架是一个组件，用于管理屏幕某一区域中的子控件、命中测试和呈现方式。 例如，Win32 窗口（通常称为 HWND）可充当一个框架，其中包含多个 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 元素，例如菜单栏、状态栏和按钮。  
  
 Win32 容器控件（例如列表框和树视图）被视为框架，因为它们包含自己的用于呈现子项和对其执行命中测试的代码。 与此相反， [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 列表框不是一个框架，因为包含 [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] 窗口处理呈现和命中测试。  
  
 应用程序中的 [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] 可由不同框架构成。 例如，HWND 应用程序窗口可能包含动态 HTML (DHTML) ，而后者又包含 HWND 中的组合框等组件。  
  
### <a name="fragments"></a>Fragments  

 片段是特定框架中元素的完整子树。 子树根节点处的元素称为片段根。 片段根不具有父级，而是承载在其他框架中，通常是 Win32 窗口 (HWND) 。  
  
### <a name="hosts"></a>主机  

 每个片段的根节点必须承载于一个元素中，通常为 Win32 窗口 (HWND) 。 例外情况是桌面，它不承载在任何其他元素中。 自定义控件的宿主是该控件本身的 HWND，而非应用程序窗口或可能包含顶级控件组的任何其他窗口。  
  
 片段的宿主对于提供 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 服务中而言非常重要。 它允许导航到片段根，并提供一些默认属性，使自定义提供程序无需实现这些属性。  
  
## <a name="see-also"></a>另请参阅

- [服务器端 UI 自动化提供程序的实现](server-side-ui-automation-provider-implementation.md)
