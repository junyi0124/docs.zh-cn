---
title: ICorProfilerCallback2 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2
helpviewer_keywords:
- ICorProfilerCallback2 interface [.NET Framework profiling]
ms.assetid: 4a261dba-450d-4f1f-8d98-865b58bfc992
topic_type:
- apiref
ms.openlocfilehash: 597a3dfecd42e206c98974093fa2417eba570f6a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729459"
---
# <a name="icorprofilercallback2-interface"></a>ICorProfilerCallback2 接口

提供公共语言运行时 (CLR) 用于在探查器订阅的事件发生时通知代码探查器的方法。 `ICorProfilerCallback2`接口是[ICorProfilerCallback](icorprofilercallback-interface.md)接口的扩展。 也就是说，它提供 .NET Framework 版本2.0 中引入的新回调。  
  
> [!NOTE]
> 每个方法实现都必须返回 HRESULT 的值 S_OK 为 "成功" 或 "E_FAIL 失败时"。 目前，CLR 将忽略每个回调返回的 HRESULT （ [ICorProfilerCallback：： ObjectReferences](icorprofilercallback-objectreferences-method.md)除外）。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[FinalizeableObjectQueued 方法](icorprofilercallback2-finalizeableobjectqueued-method.md)|通知代码探查器，具有终结器的对象已排入终结器线程，以执行其 `Finalize` 方法。|  
|[GarbageCollectionFinished 方法](icorprofilercallback2-garbagecollectionfinished-method.md)|通知探查器垃圾回收已完成，并为其发出了所有垃圾回收回调。|  
|[GarbageCollectionStarted 方法](icorprofilercallback2-garbagecollectionstarted-method.md)|通知代码探查器已开始垃圾回收。|  
|[HandleCreated 方法](icorprofilercallback2-handlecreated-method.md)|通知代码探查器已创建垃圾回收句柄。|  
|[HandleDestroyed 方法](icorprofilercallback2-handledestroyed-method.md)|通知代码探查器垃圾回收句柄已销毁。|  
|[RootReferences2 方法](icorprofilercallback2-rootreferences2-method.md)|发生垃圾回收后，通知探查器有关根引用的信息。 此方法是 [ICorProfilerCallback：： RootReferences](icorprofilercallback-rootreferences-method.md) 方法的扩展。|  
|[SurvivingReferences 方法](icorprofilercallback2-survivingreferences-method.md)|通知探查器有关垃圾回收已保留下来的对象引用。|  
|[ThreadNameChanged 方法](icorprofilercallback2-threadnamechanged-method.md)|通知代码探查器线程的名称已更改。|  
  
## <a name="remarks"></a>注解  

 CLR 在 `ICorProfilerCallback` (或) 接口中调用方法， `ICorProfilerCallback2` 以便在探查器已订阅的事件发生时通知探查器。 这是 CLR 与代码探查器进行通信时所使用的主回调接口。  
  
 代码探查器必须实现接口的方法 `ICorProfilerCallback` 。 对于 .NET Framework 2.0 及更高版本，探查器还必须实现 `ICorProfilerCallback2` 方法。 每个方法实现都必须返回 HRESULT 的值 S_OK 为 "成功" 或 "E_FAIL 失败时"。 目前，CLR 将忽略每个回调返回的 HRESULT （ [ICorProfilerCallback：： ObjectReferences](icorprofilercallback-objectreferences-method.md)除外）。  
  
 代码探查器必须在 Microsoft Windows 注册表中注册，它是实现和接口的 COM 对象 `ICorProfilerCallback` `ICorProfilerCallback2` 。 代码探查器通过调用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md)订阅要接收通知的事件。 通常在探查器的 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md)实现中完成此操作。 然后，探查器可以在事件即将发生或刚刚发生在正在执行的运行时进程中时接收来自运行时的通知。  
  
> [!NOTE]
> 探查器将注册一个 COM 对象。 如果探查器的目标 .NET Framework 版本1.0 或1.1，则该 COM 对象只需实现的方法 `ICorProfilerCallback` 。 如果目标 .NET Framework 版本2.0 及更高版本，则 COM 对象还必须实现的方法 `ICorProfilerCallback2` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback3 接口](icorprofilercallback3-interface.md)
- [ICorProfilerCallback4 接口](icorprofilercallback4-interface.md)
