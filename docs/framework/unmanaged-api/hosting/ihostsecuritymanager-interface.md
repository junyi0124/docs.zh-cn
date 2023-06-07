---
title: IHostSecurityManager 接口
ms.date: 03/30/2017
api_name:
- IHostSecurityManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager
helpviewer_keywords:
- IHostSecurityManager interface [.NET Framework hosting]
ms.assetid: c3be2cbd-2d93-438b-9888-9a0251b63c03
topic_type:
- apiref
ms.openlocfilehash: 37f606a67bef79936c81b2a36f12a00d24bd82f1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680529"
---
# <a name="ihostsecuritymanager-interface"></a>IHostSecurityManager 接口

提供允许访问和控制当前正在执行的线程的安全上下文的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetSecurityContext 方法](ihostsecuritymanager-getsecuritycontext-method.md)|从主机获取请求的 [IHostSecurityContext](ihostsecuritycontext-interface.md) 。|  
|[ImpersonateLoggedOnUser 方法](ihostsecuritymanager-impersonateloggedonuser-method.md)|请求使用当前用户标识的凭据执行代码。|  
|[OpenThreadToken 方法](ihostsecuritymanager-openthreadtoken-method.md)|打开与当前线程关联的自由访问标记。|  
|[RevertToSelf 方法](ihostsecuritymanager-reverttoself-method.md)|终止当前用户标识的模拟并返回原始线程标记。|  
|[SetSecurityContext 方法](ihostsecuritymanager-setsecuritycontext-method.md)|设置当前正在执行的线程的安全上下文。|  
|[SetThreadToken 方法](ihostsecuritymanager-setthreadtoken-method.md)|设置当前正在执行的线程的句柄。|  
  
## <a name="remarks"></a>注解  

 宿主可以通过公共语言运行时 (CLR) 和用户代码来控制对线程标记的所有代码访问。 它还可以确保在异步操作或代码点之间跨受限制的代码访问传递完整的安全上下文信息。 `IHostSecurityContext` 封装此安全上下文信息，这对于 CLR 是不透明的。  
  
 CLR 在内部处理托管线程上下文。 它在以下情况下查询特定于进程的 `IHostSecurityManager` ：  
  
- 终结器线程上的终结器执行期间。  
  
- 在类和模块构造函数执行期间。  
  
- 在工作线程上的异步点，调用 [IHostThreadPoolManager：： QueueUserWorkItem](ihostthreadpoolmanager-queueuserworkitem-method.md) 方法。  
  
- 用于 i/o 完成端口的服务。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostSecurityContext 接口](ihostsecuritycontext-interface.md)
- [承载接口](hosting-interfaces.md)
