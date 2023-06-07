---
title: 承载枚举
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged enumerations [.NET Framework], hosting
- enumerations [.NET Framework hosting]
- hosting enumerations [.NET Framework]
ms.assetid: e09131eb-1f7d-4f52-ae42-7393e9b62ef6
ms.openlocfilehash: 907b1343ddbfa28b97ac2210e28b99cd38aa6fd4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721824"
---
# <a name="hosting-enumerations"></a>承载枚举

本部分介绍宿主 API 使用的非托管枚举。  
  
## <a name="in-this-section"></a>本节内容  

 [CLSID_RESOLUTION_FLAGS 枚举](clsid-resolution-flags-enumeration.md)  
 包含指示公共语言运行时 (CLR) 应如何解析的值 `CLSID` 。  
  
 [COR_GC_STAT_TYPES 枚举](cor-gc-stat-types-enumeration.md)  
 指定要为垃圾回收记录的统计信息。  
  
 [COR_GC_THREAD_STATS_TYPES 枚举](cor-gc-thread-stats-types-enumeration.md)  
 指示线程的垃圾回收统计信息。  
  
 [EApiCategories 枚举](eapicategories-enumeration.md)  
 描述宿主可阻止在部分受信任代码中运行的功能类别。  
  
 [EBindPolicyLevels 枚举](ebindpolicylevels-enumeration.md)  
 提供指定应用或修改程序集策略的级别的标志。  
  
 [ECLRAssemblyIdentityFlags 枚举](eclrassemblyidentityflags-enumeration.md)  
 指示程序集标识的类型。  
  
 [EClrEvent 枚举](eclrevent-enumeration.md)  
 描述主机可为其注册回调的 CLR 事件。  
  
 [EClrFailure 枚举](eclrfailure-enumeration.md)  
 描述主机可对其设置策略操作的失败集。  
  
 [EClrOperation 枚举](eclroperation-enumeration.md)  
 描述主机可对其应用策略操作的一组操作。  
  
 [EClrUnhandledException 枚举](eclrunhandledexception-enumeration.md)  
 介绍用于管理用户代码中未经处理的异常的可用选项。  
  
 [EContextType 枚举](econtexttype-enumeration.md)  
 描述当前正在执行的线程的安全上下文。  
  
 [ECustomDumpFlavor 枚举](ecustomdumpflavor-enumeration.md)  
 包含一些值，这些值指示在报告错误时要包含在堆转储的自定义子集中的项。  
  
 [ECustomDumpItemKind 枚举](ecustomdumpitemkind-enumeration.md)  
 保留以供将来扩展 [CustomDumpItem 结构](customdumpitem-structure.md) 。  
  
 [EHostApplicationPolicy 枚举](ehostapplicationpolicy-enumeration.md)  
 指示如何修改 [IHostAssemblyManager 接口](ihostassemblymanager-interface.md) 接口对象。 此枚举已弃用。  
  
 [EHostBindingPolicyModifyFlags 枚举](ehostbindingpolicymodifyflags-enumeration.md)  
 允许主机在将策略修改从源程序集应用到目标程序集时指定 CLR 应执行的重定向类型。  
  
 [EInitializeNewDomainFlags 枚举](einitializenewdomainflags-enumeration.md)  
 使宿主能够向运行时提供有关应用程序域初始化的信息。  
  
 [EMemoryAvailable 枚举](ememoryavailable-enumeration.md)  
 包含指示计算机上的可用物理内存量的值。  
  
 [EMemoryCriticalLevel 枚举](ememorycriticallevel-enumeration.md)  
 包含一些值，这些值指示在请求特定内存分配但无法满足时，失败所造成的影响。  
  
 [EPolicyAction 枚举](epolicyaction-enumeration.md)  
 描述主机可为 [EClrOperation 枚举](eclroperation-enumeration.md) 描述的操作和 [EClrFailure 枚举](eclrfailure-enumeration.md)描述的故障设置的策略操作。  
  
 [ESymbolReadingPolicy 枚举](esymbolreadingpolicy-enumeration.md)  
 包含一些值，这些值设置用于读取程序数据库 (PDB) 文件的策略。  
  
 [ETaskType 枚举](etasktype-enumeration.md)  
 包含一些值，这些值指示由 [ICLRTask 接口](iclrtask-interface.md) 或 [IHostTask 接口](ihosttask-interface.md) 接口表示的任务的类型。  
  
 [HOST_TYPE 枚举](host-type-enumeration.md)  
 包含的值用于指定启动应用程序的主机的类型。  
  
 [MALLOC_TYPE 枚举](malloc-type-enumeration.md)  
 包含指定正在分配的内存特性的值。  
  
 [METAHOST_CONFIG_FLAGS 枚举](metahost-config-flags-enumeration.md)  
 描述在 `pdwConfigFlags` [ICLRMetaHostPolicy：： GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) 方法的参数中返回的可能的标志。  
  
 [METAHOST_POLICY_FLAGS 枚举](metahost-policy-flags-enumeration.md)  
 提供大多数运行时主机通用的绑定策略。  
  
 [RUNTIME_INFO_FLAGS 枚举](runtime-info-flags-enumeration.md)  
 包含指示应返回有关 CLR 的哪些信息的值。  
  
 [StackOverflowType 枚举](stackoverflowtype-enumeration.md)  
 包含指示堆栈溢出事件的根本原因的值。  
  
 [STARTUP_FLAGS 枚举](startup-flags-enumeration.md)  
 包含指示 CLR 的启动行为的值。  
  
 [ValidatorFlags 枚举](validatorflags-enumeration.md)  
 包含一些值，这些值指示应在调用 [验证方法](iclrvalidator-validate-method.md)时执行的验证类型。  
  
 [WAIT_OPTION 枚举](wait-option-enumeration.md)  
 指示 CLR 请求的操作阻止时主机应执行的操作。  
  
## <a name="related-sections"></a>相关章节  

 [承载组件类](hosting-coclasses.md)  
  
 [承载接口](hosting-interfaces.md)  
  
 [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)  
  
 [承载结构](hosting-structures.md)
