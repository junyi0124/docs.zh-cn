---
title: ICorConfiguration 接口
ms.date: 03/30/2017
api_name:
- ICorConfiguration
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorConfiguration
helpviewer_keywords:
- ICorConfiguration interface [.NET Framework hosting]
ms.assetid: aaf96116-372b-4538-afb1-9e0fcdac1f98
topic_type:
- apiref
ms.openlocfilehash: fa8d15bc8e504a57d5cc87c170a3a5b022798add
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95715779"
---
# <a name="icorconfiguration-interface"></a>ICorConfiguration 接口

提供 (CLR) 配置公共语言运行时的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AddDebuggerSpecialThread 方法](icorconfiguration-adddebuggerspecialthread-method.md)|向调试服务指示当调试器在托管或非托管调试方案中停止应用程序时，应允许特定线程继续执行。|  
|[SetDebuggerThreadControl 方法](icorconfiguration-setdebuggerthreadcontrol-method.md)|设置在阻止和取消阻止 CLR 线程以进行调试时，调试服务将调用的回调接口。|  
|[SetGCHostControl 方法](icorconfiguration-setgchostcontrol-method.md)|设置垃圾回收器用于请求主机更改虚拟内存限制的回调接口。|  
|[SetGCThreadControl 方法](icorconfiguration-setgcthreadcontrol-method.md)|设置回调接口，用于为非运行时任务计划线程，否则将阻止垃圾回收。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [CorRuntimeHost 组件类](corruntimehost-coclass.md)
