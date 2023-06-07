---
title: IHostTaskManager 接口
ms.date: 03/30/2017
api_name:
- IHostTaskManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostTaskManager
helpviewer_keywords:
- IHostTaskManager interface [.NET Framework hosting]
ms.assetid: 4a0b05b9-3ef1-4607-b7c8-bd4dd43647a0
topic_type:
- apiref
ms.openlocfilehash: deb14d291bfd511e8f3534f3c5e32787c259c5e8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673106"
---
# <a name="ihosttaskmanager-interface"></a>IHostTaskManager 接口

提供一些方法，使公共语言运行时 (CLR) 可以通过主机使用任务，而不是使用标准操作系统线程处理或纤程函数。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[BeginDelayAbort 方法](ihosttaskmanager-begindelayabort-method.md)|通知宿主托管代码输入的时间段不得中止当前任务。|  
|[BeginThreadAffinity 方法](ihosttaskmanager-beginthreadaffinity-method.md)|通知宿主托管代码正在输入一个时间段，在此期间，不能将当前任务移动到另一个操作系统线程。|  
|[CallNeedsHostHook 方法](ihosttaskmanager-callneedshosthook-method.md)|使宿主能够指定公共语言运行时是否可将指定的调用内联到非托管函数。|  
|[CreateTask 方法](ihosttaskmanager-createtask-method.md)|请求宿主创建新任务。|  
|[EndDelayAbort 方法](ihosttaskmanager-enddelayabort-method.md)|向宿主通知托管代码，在之前对的调用之后不得中止当前任务 `BeginDelayAbort` 。|  
|[EndThreadAffinity 方法](ihosttaskmanager-endthreadaffinity-method.md)|向宿主通知托管代码，即在之前对的调用之后，不能将当前任务移到另一个操作系统线程 `BeginThreadAffinity` 。|  
|[EnterRuntime 方法](ihosttaskmanager-enterruntime-method.md)|通知宿主对非托管方法的调用（如平台调用方法）正在将执行控制返回到 CLR。|  
|[GetCurrentTask 方法](ihosttaskmanager-getcurrenttask-method.md)|获取一个接口指针，该指针指向正在进行此调用的操作系统线程上当前正在执行的任务。|  
|[GetStackGuarantee 方法](ihosttaskmanager-getstackguarantee-method.md)|获取在堆栈操作完成之后但在关闭进程之前保证可用的堆栈空间量。|  
|[LeaveRuntime 方法](ihosttaskmanager-leaveruntime-method.md)|通知宿主托管代码将要对非托管函数进行调用。|  
|[ReverseEnterRuntime 方法](ihosttaskmanager-reverseenterruntime-method.md)|通知宿主从非托管代码向公共语言运行时 (CLR) 发出调用。|  
|[ReverseLeaveRuntime 方法](ihosttaskmanager-reverseleaveruntime-method.md)|通知宿主控件离开 CLR，并进入从托管代码调用的非托管函数。|  
|[SetCLRTaskManager 方法](ihosttaskmanager-setclrtaskmanager-method.md)|向宿主提供一个接口指针，该指针指向 CLR 实现的 [ICLRTaskManager](iclrtaskmanager-interface.md) 实例。|  
|[SetLocale 方法](ihosttaskmanager-setlocale-method.md)|通知宿主 CLR 已更改当前任务的区域设置。|  
|[SetStackGuarantee 方法](ihosttaskmanager-setstackguarantee-method.md)|保留以仅供内部使用。|  
|[SetUILocale 方法](ihosttaskmanager-setuilocale-method.md)|通知宿主用户界面区域设置在当前任务上已更改。|  
|[Sleep 方法](ihosttaskmanager-sleep-method.md)|通知宿主当前任务将要进入睡眠状态。|  
|[SwitchToTask 方法](ihosttaskmanager-switchtotask-method.md)|通知宿主应切换到当前任务。|  
  
## <a name="remarks"></a>注解  

 `IHostTaskManager` 允许 CLR 创建和管理任务，以便在控制从托管到非托管代码的传输时，为宿主提供挂钩，并指定在代码执行期间宿主可以和不能执行的某些操作。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRTask 接口](iclrtask-interface.md)
- [ICLRTaskManager 接口](iclrtaskmanager-interface.md)
- [IHostTask 接口](ihosttask-interface.md)
- [承载接口](hosting-interfaces.md)
