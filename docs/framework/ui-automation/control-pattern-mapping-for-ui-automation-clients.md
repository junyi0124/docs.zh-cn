---
title: UI 自动化客户端的控件模式映射
description: 查看 UI 自动化客户端的控件模式映射表。 某些控制类型的操作可能受支持、有条件支持或不受支持。
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, for UI Automation clients
- UI Automation, clients, control patterns for
ms.assetid: 8b81645b-8be3-4e26-9c98-4fb0fceca06b
ms.openlocfilehash: aaab4639a7573dd090af2e6d9bb06f896c4728f6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276550"
---
# <a name="control-pattern-mapping-for-ui-automation-clients"></a>UI 自动化客户端的控件模式映射

> [!NOTE]
> 本文档适用于想要使用 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 命名空间中定义的托管 <xref:System.Windows.Automation> 类的 .NET Framework 开发人员。 有关 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]的最新信息，请参阅 [Windows 自动化 API：UI 自动化](/windows/win32/winauto/entry-uiauto-win32)。  
  
 本主题列出了控件类型及其关联的控件模式。  
  
 下表将控件模式整理为以下类别：  
  
- 。 控件必须支持此控件模式。  
  
- 有条件支持。 控件可能支持此控件模式，具体取决于控件的状态。  
  
- 不支持。 控件不支持此控件模式；自定义控件可能支持此控件模式。  
  
> [!NOTE]
> 某些控件有条件支持一些控件模式，具体取决于控件的功能。 例如，菜单项控件有条件支持 <xref:System.Windows.Automation.InvokePattern>、 <xref:System.Windows.Automation.ExpandCollapsePattern>、 <xref:System.Windows.Automation.TogglePattern>或 <xref:System.Windows.Automation.SelectionItemPattern> 控件模式，具体取决于其菜单控件中的功能。  
  
<a name="control_mapping_clients"></a>

## <a name="ui-automation-control-patterns-for-clients"></a>客户端的 UI 自动化控件模式  
  
|控件类型|支持|有条件支持|不支持|  
|------------------|---------------|-------------------------|-------------------|  
|Button|无|调用、切换、展开折叠|无|  
|日历|网格、表|选择、滚动|值|  
|复选框|切换|无|无|  
|组合框|展开/折叠|选择、值|滚动|  
|数据网格|网格|滚动、选择、表|无|  
|数据项|选择项|展开折叠、网格项、滚动项、表、切换、值|无|  
|文档|文本|滚动、值|无|  
|编辑|无|文本、范围值、值|无|  
|组|无|展开/折叠|无|  
|标头|无|转换|无|  
|标头项|无|转换、调用|无|  
|Hyperlink|Invoke|值|无|  
|映像|无|网格项、表项|调用、选择项|  
|列出|无|网格、多个视图、滚动、选择|表|  
|列表项|选择项|展开折叠、网格项、调用、滚动项、切换、值|无|  
|菜单|无|无|无|  
|菜单栏|无|展开折叠、停靠、转换|无|  
|菜单项|无|展开折叠、调用、选择项、切换|无|  
|窗格|无|停靠。 滚动、转换|窗口|  
|进度栏|无|范围值、值|无|  
|单选按钮|选择项|无|切换|  
|滚动条|无|范围值|滚动|  
|Separator|无|无|无|  
|Slider|无|范围值、选择、值|无|  
|Spinner|无|范围值、选择、值|无|  
|拆分按钮|调用、展开折叠|无|无|  
|状态栏|无|网格|无|  
|选项卡|选项|滚动|无|  
|选项卡项|选择项|无|Invoke|  
|表|网格、网格项、表、表项|无|无|  
|文本|无|网格项、表项、文本|值|  
|Thumb|转换|无|无|  
|标题栏|无|无|无|  
|工具栏|无|停靠、展开折叠、转换|无|  
|工具提示|无|文本、窗口|无|  
|树|无|滚动、选择|无|  
|树项|展开/折叠|调用，滚动项、选择项、切换|无|  
|窗口|转换、窗口|程序坞|无|  
  
> [!NOTE]
> 如果控件类型没有可列出的受支持的控件模式，但具有一个或多个有条件支持的控件模式，则将始终支持这些有条件控件模式之一。  
  
## <a name="see-also"></a>另请参阅

- [UI 自动化概述](ui-automation-overview.md)
