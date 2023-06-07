---
title: COR_PRF_HIGH_MONITOR 枚举
ms.date: 04/10/2018
ms.assetid: 3ba543d8-15e5-4322-b6e7-1ebfc92ed7dd
ms.openlocfilehash: b813b32ef4522f1707812d337d20790ce75f3d55
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84500840"
---
# <a name="cor_prf_high_monitor-enumeration"></a>COR_PRF_HIGH_MONITOR 枚举

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
除了在[COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)枚举中找到的标记外，还提供了标志，在加载时，探查器可以为[ICorProfilerInfo5：： SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)方法指定这些标志。  
  
## <a name="syntax"></a>语法  
  
```cpp
typedef enum {  
    COR_PRF_HIGH_MONITOR_NONE                     = 0x00000000,  
    COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES          = 0x00000001,  
    COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED        = 0x00000002,
    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS = 0x00000004,
    COR_PRF_HIGH_DISABLE_TIERED_COMPILATION       = 0x00000008,
    COR_PRF_HIGH_BASIC_GC                         = 0x00000010,
    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS         = 0x00000020,
    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED    = 0x00000040,
    COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE            = 0,  
    COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH           = COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED |
                                                    COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS |
                                                    COR_PRF_HIGH_BASIC_GC |
                                                    COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS |
                                                    COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED,  
    COR_PRF_HIGH_MONITOR_IMMUTABLE                = COR_PRF_HIGH_DISABLE_TIERED_COMPILATION  
} COR_PRF_HIGH_MONITOR;  
```  
  
## <a name="members"></a>成员  
  
|成员|描述|  
|------------|-----------------|  
|`COR_PRF_HIGH_MONITOR_NONE`|不设置任何标志。|  
|`COR_PRF_HIGH_ADD_ASSEMBLY_REFERENCES`|控制在 CLR 程序集引用闭包遍历期间添加程序集引用的[ICorProfilerCallback6：： GetAssemblyReference](icorprofilercallback6-getassemblyreferences-method.md)回调。|  
|`COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED`|控制与内存中模块关联的符号流更新的[ICorProfilerCallback7：： ModuleInMemorySymbolsUpdated](icorprofilercallback7-moduleinmemorysymbolsupdated-method.md)回调。|  
|`COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS`|控制[ICorProfilerCallback9：:D ynamicmethodunloaded](icorprofilercallback9-dynamicmethodunloaded-method.md)回调，用于指示动态方法何时被垃圾回收和卸载。 <br/> [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]|
|`COR_PRF_HIGH_DISABLE_TIERED_COMPILATION`|仅适用于 .NET Core 3.0 和更高版本：禁用探查器的[分层编译](../../../core/whats-new/dotnet-core-3-0.md)。|
|`COR_PRF_HIGH_BASIC_GC`|仅适用于 .NET Core 3.0 和更高版本：提供与相同的轻型 GC 分析选项 [`COR_PRF_MONITOR_GC`](cor-prf-monitor-enumeration.md) 。 仅控制[GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)、 [GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)和[GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md)回调。 不同于 `COR_PRF_MONITOR_GC` 标志，不 `COR_PRF_HIGH_BASIC_GC` 会禁用并发垃圾回收。|
|`COR_PRF_HIGH_MONITOR_GC_MOVED_OBJECTS`|仅适用于 .NET Core 3.0 和更高版本：仅为压缩 Gc 启用[MovedReferences](icorprofilercallback-movedreferences-method.md)和[MovedReferences2](icorprofilercallback4-movedreferences2-method.md)回调。|
|`COR_PRF_HIGH_MONITOR_LARGEOBJECT_ALLOCATED`|仅限 .NET Core 3.0 及更高版本：类似于 [`COR_PRF_MONITOR_OBJECT_ALLOCATED`](cor-prf-monitor-enumeration.md) ，但仅提供有关大型对象堆（LOH）的对象分配的信息。|
|`COR_PRF_HIGH_REQUIRE_PROFILE_IMAGE`|表示需要配置增强的映像的所有 `COR_PRF_HIGH_MONITOR` 标志。 它对应于 `COR_PRF_REQUIRE_PROFILE_IMAGE` [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)枚举中的标志。|  
|`COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH`|表示可以在将探查器附加到运行中的应用之后进行设置的所有 `COR_PRF_HIGH_MONITOR` 标志。|  
|`COR_PRF_HIGH_MONITOR_IMMUTABLE`|表示只能在初始化过程中进行设置的所有 `COR_PRF_HIGH_MONITOR` 标志。 如果尝试从其他位置更改这些标志中的任何一个标志，则会产生一个指示失败的 `HRESULT` 值。|  
  
## <a name="remarks"></a>注解

`COR_PRF_HIGH_MONITOR`标志与 `pdwEventsHigh` [ICorProfilerInfo5：： GetEventMask2](icorprofilerinfo5-geteventmask2-method.md)和[ICorProfilerInfo5：： SetEventMask2](icorprofilerinfo5-seteventmask2-method.md)方法的参数一起使用。  
  
从 .NET Framework 4.6.1 开始，的值 `COR_PRF_HIGH_ALLOWABLE_AFTER_ATTACH` 从0更改为 `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` （0x00000002）。 从 .NET Framework 4.7.2 开始，其值从更改 `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED` 为 `COR_PRF_HIGH_IN_MEMORY_SYMBOLS_UPDATED | COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS` 。

`COR_PRF_HIGH_MONITOR_IMMUTABLE`旨在作为位掩码，表示只能在初始化期间设置的所有标志。 尝试在其他任何位置更改这些标志会导致失败 `HRESULT` 。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
  
**头文件：** CorProf.idl、CorProf.h  
  
**库：** CorGuids.lib  
  
**.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
- [COR_PRF_MONITOR 枚举](cor-prf-monitor-enumeration.md)
- [ICorProfilerInfo5 接口](icorprofilerinfo5-interface.md)
