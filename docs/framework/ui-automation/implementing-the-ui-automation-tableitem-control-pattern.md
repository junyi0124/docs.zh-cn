---
title: 实现 UI 自动化 TableItem 控件模式
description: 在 UI 自动化中查看实现 TableItem 控件模式的准则和约定。 了解 ITableItemProvider 接口的必需成员。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Table Item
- UI Automation, Table Item control pattern
- TableItem control pattern
ms.assetid: ac178408-1485-436f-8d3e-eee3bf80cb24
ms.openlocfilehash: 5e83f68772de3026fe8bcb265a11999e0b85a164
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265669"
---
# <a name="implementing-the-ui-automation-tableitem-control-pattern"></a>实现 UI 自动化 TableItem 控件模式

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题介绍了实现 <xref:System.Windows.Automation.Provider.ITableItemProvider>的准则和约定，包括有关事件和属性的信息。 本概述的结尾列出了指向其他参考资料的链接。  
  
 <xref:System.Windows.Automation.TableItemPattern> 控件模式用于支持实现 <xref:System.Windows.Automation.Provider.ITableProvider> 的容器的子控件。 对个别单元格功能的访问是由 <xref:System.Windows.Automation.Provider.IGridItemProvider> 必需的并发实现提供的。 此控件模式类似于 <xref:System.Windows.Automation.Provider.IGridItemProvider>，不同之处在于任何实现 <xref:System.Windows.Automation.Provider.ITableItemProvider> 的控件都必须以编程方式公开各个单元格与其行和列信息之间的关系。 有关实现此控件模式的控件示例，请参阅 [Control Pattern Mapping for UI Automation Clients](control-pattern-mapping-for-ui-automation-clients.md)。  
  
<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>实现准则和约定  
  
- 有关相关的网格项功能，请参阅 [实现 UI 自动化 GridItem 控件模式](implementing-the-ui-automation-griditem-control-pattern.md)。  
  
<a name="Required_Members_for_ITableItemProvider"></a>

## <a name="required-members-for-itableitemprovider"></a>ITableItemProvider 必需的成员  
  
|必需的成员|成员类型|说明|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider.GetColumnHeaderItems%2A>|方法|无|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider.GetRowHeaderItems%2A>|方法|无|  
  
 没有与此控件模式关联的属性或事件。  
  
<a name="Exceptions"></a>

## <a name="exceptions"></a>异常  

 没有与此控件模式关联的异常。  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化控件模式概述](ui-automation-control-patterns-overview.md)
- [在 UI 自动化提供程序中支持控件模式](support-control-patterns-in-a-ui-automation-provider.md)
- [客户端的 UI 自动化控件模式](ui-automation-control-patterns-for-clients.md)
- [实现 UI 自动化 Table 控件模式](implementing-the-ui-automation-table-control-pattern.md)
- [实现 UI 自动化 GridItem 控件模式](implementing-the-ui-automation-griditem-control-pattern.md)
- [UI 自动化树概述](ui-automation-tree-overview.md)
- [在 UI 自动化中使用缓存](use-caching-in-ui-automation.md)
