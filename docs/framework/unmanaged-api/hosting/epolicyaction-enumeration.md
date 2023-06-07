---
title: EPolicyAction 枚举
ms.date: 03/30/2017
api_name:
- EPolicyAction
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EPolicyAction
helpviewer_keywords:
- EPolicyAction enumeration [.NET Framework hosting]
ms.assetid: 72dd76ba-239e-45ac-9ded-318fb07d6c6d
topic_type:
- apiref
ms.openlocfilehash: 72b371d72b2f055f2840da5595d9022ffd7e2507
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674724"
---
# <a name="epolicyaction-enumeration"></a>EPolicyAction 枚举

描述主机可为 [EClrOperation](eclroperation-enumeration.md) 描述的操作设置的策略操作以及 [EClrFailure](eclrfailure-enumeration.md)描述的故障。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    eNoAction,  
    eThrowException,  
    eAbortThread,  
    eRudeAbortThread,  
    eUnloadAppDomain,  
    eRudeUnloadAppDomain,  
    eExitProcess,  
    eFastExitProcess,  
    eRudeExitProcess,  
    eDisableRuntime  
} EPolicyAction;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`eAbortThread`|指定公共语言运行时 (CLR) 应正常中止线程。 正常中止包括尝试运行所有 `finally` 块、 `catch` 与线程中止相关的任何块和终结器。|  
|`eDisableRuntime`|指定 CLR 应进入禁用状态。 在受影响的进程中，不能再执行其他托管代码，并且阻止线程进入 CLR。|  
|`eExitProcess`|指定 CLR 应尝试正常退出进程，包括运行终结器并执行清理和日志记录操作。|  
|`eFastExitProcess`|指定 CLR 应立即退出进程，而无需运行终结器或执行清理和日志记录操作。 但是，通知将发送到调试器。|  
|`eNoAction`|指定不执行任何操作。|  
|`eRudeAbortThread`|指定 CLR 应执行强制的线程中止。 只 `catch` `finally` 会执行标记为的和块 <xref:System.EnterpriseServices.MustRunInClientContextAttribute> 。|  
|`eRudeExitProcess`|指定 CLR 应退出进程，而不运行终结器或日志记录操作。|  
|`eRudeUnloadAppDomain`|指定 CLR 应执行的强制卸载 <xref:System.AppDomain> 。 仅执行标记为的终结器 <xref:System.EnterpriseServices.MustRunInClientContextAttribute> 。 同样，在其堆栈中具有此类的所有线程都 <xref:System.AppDomain> 将接收 `ThreadAbortException` ，但仅 `catch` 执行标记为的和 `finally` 块 <xref:System.EnterpriseServices.MustRunInClientContextAttribute> 。|  
|`eThrowException`|指定应引发适用于条件的异常，例如内存不足、缓冲区溢出，等等。|  
|`eUnloadAppDomain`|指定 <xref:System.AppDomain> 应卸载。 CLR 尝试运行终结器。|  
  
## <a name="remarks"></a>注解  

 宿主通过调用 [ICLRPolicyManager](iclrpolicymanager-interface.md) 接口的方法来设置策略操作。 有关 "强制" 和 "正常中止" 的信息，请参阅 [EClrOperation](eclroperation-enumeration.md) 枚举。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrFailure 枚举](eclrfailure-enumeration.md)
- [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)
- [IHostPolicyManager 接口](ihostpolicymanager-interface.md)
- [承载枚举](hosting-enumerations.md)
