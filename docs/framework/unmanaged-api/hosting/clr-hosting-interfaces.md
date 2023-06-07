---
title: CLR 承载接口
ms.date: 03/30/2017
helpviewer_keywords:
- interfaces [.NET Framework hosting], version 2.0
- hosting interfaces [.NET Framework], version 2.0
- .NET Framework 2.0, hosting interfaces
ms.assetid: 703b8381-43db-4a4d-9faa-cca39302d922
ms.openlocfilehash: 77f2ba64d9bdbe9793d56e88dae46fd506119ab8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719042"
---
# <a name="clr-hosting-interfaces"></a>CLR 承载接口

本部分介绍了非托管主机可用于将公共语言运行时 (CLR) 集成到其应用程序的接口。 此信息适用于2.0 版及更高版本的 .NET Framework。 这些接口使宿主能够控制运行时的更多方面，而不是在版本1.0 和1.1 中提供的，并且在 CLR 与宿主的执行模型之间提供了更紧密的集成。  
  
 在 .NET Framework 版本1.0 和1.1 中，托管模型启用了一个非托管主机，用于将 CLR 加载到进程中、配置特定设置以及接收事件通知。 不过，通常情况下，主机和 CLR 在该进程中单独运行。 在 .NET Framework 版本2.0 及更高版本中，新的抽象层允许宿主提供当前由 Win32 程序集中的类型提供的多个资源，并扩展主机可配置的功能集。  
  
## <a name="in-this-section"></a>本节内容  

 [IActionOnCLREvent 接口](iactiononclrevent-interface.md)  
 提供一个方法，该方法对已注册的事件执行回调。  
  
 [IApartmentCallback 接口](iapartmentcallback-interface.md)  
 提供用于在单元中进行回调的方法。  
  
 [IAppDomainBinding 接口](iappdomainbinding-interface.md)  
 提供设置运行时配置的方法。  
  
 [ICatalogServices 接口](icatalogservices-interface.md)  
 提供用于编录服务的方法。  (此接口支持 .NET Framework 基础结构，不应在代码中直接使用。 )   
  
 [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)  
 提供一些方法，这些方法支持宿主与 CLR 之间的有关程序集的通信。  
  
 [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)  
 管理由 CLR （而不是由主机）加载的程序集的列表。  
  
 [ICLRControl 接口](iclrcontrol-interface.md)  
 为宿主提供方法，以获取和配置 CLR 的各个方面。  
  
 [ICLRDebugManager 接口](iclrdebugmanager-interface.md)  
 提供使宿主可以将一组任务与标识符和友好名称关联的方法。  
  
 [ICLRErrorReportingManager 接口](iclrerrorreportingmanager-interface.md)  
 提供使宿主可以配置自定义堆转储以用于错误报告的方法。  
  
 [ICLRGCManager 接口](iclrgcmanager-interface.md)  
 提供使宿主能够与 CLR 的垃圾回收系统交互的方法。  
  
 [ICLRHostBindingPolicyManager 接口](iclrhostbindingpolicymanager-interface.md)  
 为宿主提供方法，用于计算和传达程序集策略信息中的更改。  
  
 [ICLRHostProtectionManager 接口](iclrhostprotectionmanager-interface.md)  
 允许宿主阻止在部分受信任的代码中运行特定的托管类、方法、属性和字段。  
  
 [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)  
 实现一个回调方法，该方法使宿主能够向 CLR 通知指定 i/o 请求的状态。  
  
 [ICLRMemoryNotificationCallback 接口](iclrmemorynotificationcallback-interface.md)  
 允许主机使用类似于 Win32 函数的方法来报告内存压力情况 `CreateMemoryResourceNotification` 。  
  
 [ICLROnEventManager 接口](iclroneventmanager-interface.md)  
 提供使宿主能够注册和注销 CLR 事件的回调的方法。  
  
 [ICLRPolicyManager 接口](iclrpolicymanager-interface.md)  
 提供使宿主可以指定在发生故障和超时时要执行的策略操作的方法。  
  
 [ICLRProbingAssemblyEnum 接口](iclrprobingassemblyenum-interface.md)  
 提供使宿主可以通过使用 CLR 内部的程序集的标识信息来获取程序集的探测标识的方法，无需创建或了解该标识。  
  
 [ICLRReferenceAssemblyEnum 接口](iclrreferenceassemblyenum-interface.md)  
 提供一些方法，这些方法使宿主可以使用 CLR 内部的程序集标识数据来操作由文件或流引用的程序集集，而无需创建或了解这些标识。  
  
 [ICLRRuntimeHost 接口](iclrruntimehost-interface.md)  
 提供类似于 [ICorRuntimeHost](icorruntimehost-interface.md)的功能，另外还提供了一个用于设置宿主控件接口的方法。  
  
 [ICLRSyncManager 接口](iclrsyncmanager-interface.md)  
 为宿主提供方法，以获取有关请求任务的信息，并在其同步实现中检测死锁。  
  
 [ICLRTask 接口](iclrtask-interface.md)  
 提供一些方法，这些方法使宿主可以发出 CLR 请求，或向 CLR 提供有关关联任务的通知。  
  
 [ICLRTaskManager 接口](iclrtaskmanager-interface.md)  
 提供使宿主能够显式请求 CLR 创建新任务、获取当前正在执行的任务以及设置任务的地理语言和区域性的方法。  
  
 [ICLRValidator 接口](iclrvalidator-interface.md)  
 提供用于验证 (PE) 映像和报告验证错误的可移植可执行文件的方法。  
  
 [ICorConfiguration 接口](icorconfiguration-interface.md)  
 提供配置 CLR 的方法。  
  
 [ICorThreadpool 接口](icorthreadpool-interface.md)  
 提供用于访问线程池的方法。  
  
 [IDebuggerInfo 接口](idebuggerinfo-interface.md)  
 提供用于获取有关调试服务状态的信息的方法。  
  
 [IDebuggerThreadControl 接口](idebuggerthreadcontrol-interface.md)  
 提供一些方法，用于通知宿主调试服务阻止和取消阻止线程。  
  
 [IGCHost 接口](igchost-interface.md)  
 提供一些方法，用于获取有关垃圾回收系统的信息并控制垃圾回收的某些方面。  
  
 [IGCHost2 接口](igchost2-interface.md)  
 提供 [SetGCStartupLimitsEx](igchost2-setgcstartuplimitsex-method.md) 方法，该方法使宿主可以将垃圾回收段的大小和垃圾回收系统的代零的最大大小设置为大于的值 `DWORD` 。  
  
 [IGCHostControl 接口](igchostcontrol-interface.md)  
 提供一个方法，该方法使垃圾回收器能够请求宿主更改虚拟内存的限制。  
  
 [IGCThreadControl 接口](igcthreadcontrol-interface.md)  
 提供一些方法，这些方法用于参与要阻止垃圾回收的线程的计划。  
  
 [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)  
 提供使宿主可以指定应由 CLR 或由主机加载的程序集的方法。  
  
 [IHostAssemblyStore 接口](ihostassemblystore-interface.md)  
 提供使宿主可以独立于 CLR 加载程序集和模块的方法。  
  
 [IHostAutoEvent 接口](ihostautoevent-interface.md)  
 提供宿主实现的自动重置事件的表示形式。  
  
 [IHostControl 接口](ihostcontrol-interface.md)  
 提供用于配置程序集加载和确定宿主支持哪些宿主接口的方法。  
  
 [IHostCrst 接口](ihostcrst-interface.md)  
 用作线程的关键部分的宿主表示形式。  
  
 [IHostGCManager 接口](ihostgcmanager-interface.md)  
 提供一些方法，这些方法可向宿主通知 CLR 实现的垃圾回收机制中的事件。  
  
 [IHostIoCompletionManager 接口](ihostiocompletionmanager-interface.md)  
 提供使 CLR 能够与主机提供的 i/o 完成端口进行交互的方法。  
  
 [IHostMalloc 接口](ihostmalloc-interface.md)  
 为 CLR 提供一些方法，用于通过宿主请求从堆中进行细粒度分配。  
  
 [IHostManualEvent 接口](ihostmanualevent-interface.md)  
 提供宿主手动重置事件表示形式的实现。  
  
 [IHostMemoryManager 接口](ihostmemorymanager-interface.md)  
 为 CLR 提供一些方法，用于通过主机进行虚拟内存请求，而不是使用标准 Win32 虚拟内存函数。  
  
 [IHostPolicyManager 接口](ihostpolicymanager-interface.md)  
 提供一些方法，这些方法在发生中止、超时或失败时通知 CLR 执行的操作。  
  
 [IHostSecurityContext 接口](ihostsecuritycontext-interface.md)  
 使 CLR 能够维护宿主实现的安全上下文信息。  
  
 [IHostSecurityManager 接口](ihostsecuritymanager-interface.md)  
 提供允许访问和控制当前正在执行的线程的安全上下文的方法。  
  
 [IHostSemaphore 接口](ihostsemaphore-interface.md)  
 提供由主机实现的信号量的表示形式。  
  
 [IHostSyncManager 接口](ihostsyncmanager-interface.md)  
 为 CLR 提供方法，以通过调用主机而不是使用 Win32 同步函数来创建同步基元。  
  
 [IHostTask 接口](ihosttask-interface.md)  
 提供使 CLR 能够与宿主通信以管理任务的方法。  
  
 [IHostTaskManager 接口](ihosttaskmanager-interface.md)  
 提供一些方法，使 CLR 可以通过主机使用任务，而不是使用标准的操作系统线程或纤程函数。  
  
 [IHostThreadPoolManager 接口](ihostthreadpoolmanager-interface.md)  
 为 CLR 提供方法来配置线程池并将工作项排队到线程池。  
  
 [IManagedObject 接口](imanagedobject-interface.md)  
 提供用于控制托管对象的方法。  
  
 IObjectHandle  
 提供了一种方法，用于从间接寻址解包按值封送对象。  
  
 [ITypeName 接口](itypename-interface.md)  
 提供用于获取类型名称信息的方法。  (此接口支持 .NET Framework 基础结构，不应在代码中直接使用。 )   
  
 [ITypeNameBuilder 接口](itypenamebuilder-interface.md)  
 提供用于生成类型名称的方法。  (此接口支持 .NET Framework 基础结构，不应在代码中直接使用。 )   
  
 [ITypeNameFactory 接口](itypenamefactory-interface.md)  
 提供用于析构类型名称的方法。  (此接口支持 .NET Framework 基础结构，不应在代码中直接使用。 )   
  
 IValidator  
 提供用于验证 (PE) 映像和报告验证错误的可移植可执行文件的方法。  
  
## <a name="related-sections"></a>相关章节  

 [弃用的 CLR 承载接口和 Coclass](deprecated-clr-hosting-interfaces-and-coclasses.md)  
 包含描述 .NET Framework 版本1.0 和1.1 中提供的托管接口的主题。  
  
 [.NET Framework 4 和 4.5 中添加的 CLR 承载接口](clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)  
 包含描述 .NET Framework 4 中提供的宿主接口的主题。
