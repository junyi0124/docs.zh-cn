---
title: 实现 UI 自动化 MultipleView 控件模式
description: 在 UI 自动化中查看实现 MultipleView 控件模式的准则和约定。 请参阅 IMultipleViewProvider 接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, MultipleView control pattern
- MultipleView control pattern
- control patterns, MultipleView
ms.assetid: 5bf1b248-ffee-48c8-9613-0b134bbe9f6a
ms.openlocfilehash: 7e64a696e8dc96123631853b06ea3ccee434d2f1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239428"
---
# <a name="implementing-the-ui-automation-multipleview-control-pattern"></a>实现 UI 自动化 MultipleView 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍了实现 <xref:System.Windows.Automation.Provider.IMultipleViewProvider>的准则和约定，包括有关事件和属性的信息。 本主题的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.MultipleViewPattern> 控件模式用于支持那些提供并能够在同组信息或子控件的多个表示形式间进行切换的控件。  
  
 可显示多个视图的控件示例包括列表视图 (可以将其内容显示为缩略图，磁贴、图标或详细信息) ，Microsoft Excel 图表 (饼图、行、栏、单元格值和公式) 、Microsoft Word 文档 (普通、Web 版式、打印布局、阅读版式、大纲) 、Microsoft Outlook 日历 (年、月、周、天) 和 Microsoft Windows Media Player 外观。 支持的视图由控件开发人员确定，并特定于每个控件。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  

 在实现 Multiple View 控件模式时，请注意以下准则和约定：  
  
- <xref:System.Windows.Automation.Provider.IMultipleViewProvider> 还应在一个容器中实现，此容器用于管理当前视图，如果它不同于提供当前视图的控件。 例如，Windows 资源管理器包含当前文件夹内容的列表控件，而该控件的视图是从 Windows 资源管理器应用程序进行管理的。  
  
- 能够对其内容进行排序的控件不被视为支持多个视图。  
  
- 视图的集合必须跨实例相同。  
  
- 视图名称必须是适合在文本到语音转换、盲文和其他用户可读的应用程序中使用。  
  
<a name="Required_Members_for_IMultipleViewProvider"></a>

## <a name="required-members-for-imultipleviewprovider"></a>IMultipleViewProvider 所需的成员  

 实现 IMultipleViewProvider 需要以下属性和方法。  
  
|必需的成员|成员类型|说明|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.CurrentView%2A>|属性|无|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetSupportedViews%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A>|方法|无|  
  
 没有与此控件模式相关联的事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 提供程序必须引发以下异常。  
  
|例外类型|条件|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|当 <xref:System.Windows.Automation.Provider.IMultipleViewProvider.SetCurrentView%2A> 或 <xref:System.Windows.Automation.Provider.IMultipleViewProvider.GetViewName%2A> 与不是受支持视图集合成员的参数一同被调用时。|  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
