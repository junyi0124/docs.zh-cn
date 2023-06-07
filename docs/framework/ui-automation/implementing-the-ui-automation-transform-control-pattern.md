---
title: 实现 UI 自动化 Transform 控件模式
description: 在 UI 自动化中查看实现 "转换" 控件模式的准则和约定。 了解 Itransformprovider 必需接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Transform
- Transform control pattern
- UI Automation, Transform control pattern
ms.assetid: 5f49d843-5845-4800-9d9c-56ce0d146844
ms.openlocfilehash: fc47170a08ff08f6cd8f67996ef8fbf19c40f819
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265643"
---
# <a name="implementing-the-ui-automation-transform-control-pattern"></a>实现 UI 自动化 Transform 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍实现 <xref:System.Windows.Automation.Provider.ITransformProvider>的准则和约定，包括有关属性、方法和事件的信息。 本主题的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.TransformPattern> 控件模式用于支持可以移动、调整大小或在二维空间中旋转的控件。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现 Transform 控件模式时，请注意以下准则和约定：  
  
- 对此控件模式的支持并不限于桌面上的对象。 如果子级可以移动、调整大小或在容器的边界内自由地旋转，则此控件模式还必须受到容器对象子级的支持。  
  
- 如果移动、旋转对象或调整其大小使得屏幕位置完全处于其容器的坐标之外（例如，当顶层窗口移动到屏幕之外或子对象移动到容器的视区边界之外时），结果导致键盘或鼠标无法访问，则不能如此操作。 在这些情况下，对象被放在尽可能靠近所请求的屏幕坐标位置，而顶部或左侧坐标被覆盖以位于容器边界内。  
  
- 对于多监视器系统，如果一个对象被移动、调整大小或旋转导致完全位于组合桌面屏幕坐标外，则该对象被放置在尽可能靠近所请求坐标的主监视器中。  
  
- 所有参数和属性值都是绝对和独立于区域设置的。  
  
<a name="Required_Members_for_the_IValueProvider_Interface"></a>

## <a name="required-members-for-itransformprovider"></a>ITransformProvider 必需的成员  

 实现 <xref:System.Windows.Automation.Provider.ITransformProvider>需要以下属性和方法。  
  
|必需的成员|成员类型|说明|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanMove%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanResize%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.CanRotate%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Move%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Resize%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.ITransformProvider.Rotate%2A>|方法|无|  
  
 没有与此控件模式关联的事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|异常类型|条件|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Move%2A><br /><br /> -如果 <xref:System.Windows.Automation.TransformPatternIdentifiers.CanMoveProperty> 为 false。|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Resize%2A><br /><br /> -如果 <xref:System.Windows.Automation.TransformPatternIdentifiers.CanResizeProperty> 为 false。|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.ITransformProvider.Rotate%2A><br /><br /> -如果 <xref:System.Windows.Automation.TransformPatternIdentifiers.CanRotateProperty> 为 false。|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
