---
title: System.ServiceModel.ManualFlowThrottleLimitReached
ms.date: 03/30/2017
ms.assetid: 9aba851f-1830-493b-8e3e-60f454eb923e
ms.openlocfilehash: 180a06efc94acba40806e1f5d661553127549596
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248482"
---
# <a name="systemservicemodelmanualflowthrottlelimitreached"></a>System.ServiceModel.ManualFlowThrottleLimitReached

System.ServiceModel.ManualFlowThrottleLimitReached  
  
## <a name="description"></a>描述  

 系统已达到为 ManualFlowControlLimit throttle 阈值设置的限制。 通过对 ServiceHost 或 InstanceContext 上的 ManualFlowControlLimit 属性进行适当修改，可以更改该阈值。  
  
 在手动流控制限制最初减少为 0 时发出此跟踪。 不跟踪后续更改为 0 的情况。 对于每个上下文只跟踪一次实例上下文上的流控制限制。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
