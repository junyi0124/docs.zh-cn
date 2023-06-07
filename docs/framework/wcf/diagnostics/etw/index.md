---
title: 使用 ETW 进行分析跟踪
ms.date: 03/30/2017
helpviewer_keywords:
- diagnostics [WCF], analytic tracing
- administration [WCF], analytic tracing
- analytic tracing [WCF]
ms.assetid: 1d518e47-a38d-41e8-93d7-8c3b361f6a56
ms.openlocfilehash: 4bb31eeac7c5d3c8c30f66090b07de9f8af4d5a4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274616"
---
# <a name="analytic-tracing-with-etw"></a>使用 ETW 进行分析跟踪

Windows Communication Foundation (WCF) 分析跟踪提供一种在执行 WCF 服务的过程中捕获诊断信息的方法。 在 WCF 堆栈中的关键点处发出 WCF 分析跟踪事件，以允许在生产环境中对 WCF 服务进行故障排除。 WCF 服务的分析跟踪对承载 wcf 服务的产品服务器的性能的影响最小， [!INCLUDE[netfx_current_long](../../../../../includes/netfx-current-long-md.md)] 因为这些事件非常有效地发出到 Windows (ETW) 会话的事件跟踪。  
  
## <a name="in-this-section"></a>本节内容  

 [分析跟踪概述](analytic-tracing-overview.md)  
 讨论 WCF 分析跟踪在中的工作方式 [!INCLUDE[netfx_current_long](../../../../../includes/netfx-current-long-md.md)] 。  
  
 [动态启用分析跟踪](dynamically-enabling-analytic-tracing.md)  
 讨论如何使用 ETW 动态启用或禁用跟踪。  
  
 [配置消息流跟踪](configuring-message-flow-tracing.md)  
 描述如何配置消息流跟踪。  
  
 [分析跟踪事件参考](analytic-trace-event-reference.md)  
 显示一张表，其中包括事件 ID 及其事件级别、事件消息和关键字。  
  
## <a name="see-also"></a>另请参阅

- [针对 Windows 的 WCF 服务和事件跟踪](../../samples/wcf-services-and-event-tracing-for-windows.md)
- [在 Windows 事件跟踪中跟踪事件](../../../windows-workflow-foundation/samples/tracking-events-into-event-tracing-in-windows.md)
