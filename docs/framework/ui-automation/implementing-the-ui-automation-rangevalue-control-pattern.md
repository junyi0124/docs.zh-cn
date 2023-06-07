---
title: 实现 UI 自动化 RangeValue 控件模式
description: 查看在 UI 自动化中实现 RangeValue 控件模式的准则和约定。 请参阅 Irangevalueprovider 必需接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Range Value
- Range Value control pattern
- UI Automation, Range Value control pattern
ms.assetid: 225feaa4-918e-418b-938e-7389338d0a69
ms.openlocfilehash: 9b5bfd571b078b7aeab149f5371004ac832fadcc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239557"
---
# <a name="implementing-the-ui-automation-rangevalue-control-pattern"></a>实现 UI 自动化 RangeValue 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍了实现 <xref:System.Windows.Automation.Provider.IRangeValueProvider>的准则和约定，包括有关事件和属性的信息。 本主题的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.RangeValuePattern> 控件模式用于支持可被设置为范围内的某个值的控件。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现 Range Value 控件模式时，请注意以下准则和约定：  
  
- 控件允许基于区域设置或用户首选项来校准支持的属性。 这样的一个示例是温度计控件，它可被设置为以华氏或摄氏显示温度。  
  
- 具有不明确范围值的控件（如进度栏或滑块）应对这些值进行规范化。  
  
 ![进度栏。](./media/uia-rangevaluepattern-progress-bar.PNG "UIA_RangeValuePattern_Progress_Bar")  
进度栏的示例，其中值为整数类型，最小和最大属性值分别被规范化为 0 和 100  
  
<a name="Required_Members_for_the_IRangeValueProvider"></a>

## <a name="required-members-for-irangevalueprovider"></a>IRangeValueProvider 必需的成员  
  
|必需的成员|成员类型|说明|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.RangeValuePattern.IsReadOnlyProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.ValueProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.LargeChangeProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.SmallChangeProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.MaximumProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.MinimumProperty>|属性|无|  
|<xref:System.Windows.Automation.RangeValuePattern.SetValue%2A>|方法|无|  
  
 没有与此控件模式关联的事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|例外类型|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentOutOfRangeException>|使用一个大于<xref:System.Windows.Automation.RangeValuePattern.SetValue%2A> 或小于 <xref:System.Windows.Automation.RangeValuePattern.MaximumProperty> 的值调用 <xref:System.Windows.Automation.RangeValuePattern.MinimumProperty>。|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
