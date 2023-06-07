---
title: 实现 UI 自动化 ScrollItem 控件模式
description: 查看在 UI 自动化中实现 ScrollItem 控件模式的准则和约定。 请参阅 IScrollItemProvider 接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Scroll Item
- UI Automation, Scroll Item control pattern
- Scroll Item control pattern
ms.assetid: 903bab5c-80c1-44d7-bdc2-0a418893b987
ms.openlocfilehash: 3274f75af0ca055547a75fd8db6384ddb0f59483
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239232"
---
# <a name="implementing-the-ui-automation-scrollitem-control-pattern"></a>实现 UI 自动化 ScrollItem 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍了实现 <xref:System.Windows.Automation.Provider.IScrollItemProvider> 的准则和约定，包括有关属性、方法和事件的信息。 本主题的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.ScrollItemPattern> 控件模式用于支持实现 <xref:System.Windows.Automation.Provider.IScrollProvider> 的容器的各个子控件。 此控件模式充当子控件与其容器之间的通信通道，以确保容器可以更改其视区内当前可见的内容（或区域）以显示子控件。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现“滚动项”控件模式时，请注意以下准则和约定：  
  
- 包含在 Window 或 Canvas 控件内的项不需要实现 IScrollItemProvider 接口。 但是作为替代方法，它们必须公开 <xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 的一个有效位置。 这将允许 UI 自动化客户端应用程序使用容器上的 <xref:System.Windows.Automation.ScrollPattern> 控件模式方法，以显示子项。  
  
<a name="Required_Members_for_IScrollItemProvider"></a>

## <a name="required-members-for-iscrollitemprovider"></a>IScrollItemProvider 必需的成员  

 需要以下方法来实现 IScrollProvider 接口。  
  
|必需的成员|成员类型|说明|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IScrollItemProvider.ScrollIntoView%2A>|-方法|无|  
  
 没有与此控件模式关联的属性或事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|异常类型|条件|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|如果项无法滚动到视图：<br /><br /> -   <xref:System.Windows.Automation.ScrollItemPattern.ScrollIntoView%2A>|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
