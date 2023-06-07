---
title: 实现 UI 自动化 Scroll 控件模式
description: 查看在 UI 自动化中实现 Scroll 控件模式的准则和约定。 请参阅 Iscrollprovider 必需接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Scroll control pattern
- control patterns, Scroll
- Scroll control pattern
ms.assetid: 73d64242-6cbb-424c-92dd-dc69530b7899
ms.openlocfilehash: 4069772530d8b4db817aa1b7a9be86a3ee83881e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239258"
---
# <a name="implementing-the-ui-automation-scroll-control-pattern"></a>实现 UI 自动化 Scroll 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍了实现 <xref:System.Windows.Automation.Provider.IScrollProvider>的准则和约定，包括有关事件和属性的信息。 本主题的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.ScrollPattern> 控件模式用于支持充当子对象集合的可滚动容器的控件。 尽管通常需要使用滚动条来支持滚动功能，但该控件并不需要。  
  
 ![无滚动栏的滚动控件。](./media/uia-scrollpattern-without-scrollbars.PNG "UIA_ScrollPattern_Without_Scrollbars")  
不使用滚动条的滚动控件示例  
  
 有关实现此控件的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现 Scroll 控件模式时，请注意以下准则和约定：  
  
- 此控件的子级必须实现 <xref:System.Windows.Automation.Provider.IScrollItemProvider>。  
  
- 容器控件的滚动条不支持 <xref:System.Windows.Automation.ScrollPattern> 控件模式。 相反，它们必须支持 <xref:System.Windows.Automation.RangeValuePattern> 控件模式。  
  
- 当以百分比度量滚动时，与滚动刻度相关的所有值或量必须规范化为 0 到 100 的范围。  
  
- <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> 和 <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> 独立于 <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>。  
  
- 如果 <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty> = `false` 则 <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty> 应设置为 100%，且 <xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty> 应设置为 <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll>。 类似，如果 <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty> = `false` 则 <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty> 应设置为 100%，且 <xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty> 应设置为 <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll>。 这样一来，UI 自动化客户端就可以在该方法中使用这些属性值， <xref:System.Windows.Automation.ScrollPattern.SetScrollPercent%2A> 同时避免 [争用条件](https://support.microsoft.com/default.aspx?scid=kb;en-us;317723) （如果客户端不感兴趣地激活）。  
  
- <xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalScrollPercent%2A> 是特定于区域设置。 设置 HorizontalScrollPercent = 100.0 时，则对于英语等从左到右阅读的语言，必须将控件的滚动位置设置为等于其最右位置。 而对于阿拉伯语等从右到左阅读的语言，设置 HorizontalScrollPercent = 100.0 时必须将滚动位置设置为最左位置。  
  
<a name="Required_Members_for_IScrollProvider"></a>

## <a name="required-members-for-iscrollprovider"></a>IScrollProvider 必需的成员  

 实现 <xref:System.Windows.Automation.Provider.IScrollProvider>需要以下属性和方法。  
  
|必需的成员|成员类型|说明|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalScrollPercent%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticalScrollPercent%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontalViewSize%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticalViewSize%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.HorizontallyScrollable%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.VerticallyScrollable%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A>|方法|无|  
  
 没有与此控件模式关联的事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|异常类型|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|如果控件仅对于水平或垂直滚动支持<xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A> 值，而传入了 <xref:System.Windows.Automation.ScrollAmount.SmallIncrement> 值，则 <xref:System.Windows.Automation.ScrollAmount.LargeIncrement> 将引发此异常。|  
|<xref:System.ArgumentException>|当传入值无法转换为双精度时，<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A> 会引发此异常。|  
|<xref:System.ArgumentOutOfRangeException>|当传入大于 100 或小于 0 的值（-1 除外，因为它等效于<xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A> ）时， <xref:System.Windows.Automation.ScrollPatternIdentifiers.NoScroll>会引发此异常。|  
|<xref:System.InvalidOperationException>|当尝试在不支持的方向进行滚动时， <xref:System.Windows.Automation.Provider.IScrollProvider.Scroll%2A> 和 <xref:System.Windows.Automation.Provider.IScrollProvider.SetScrollPercent%2A> 都引发此异常。|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
