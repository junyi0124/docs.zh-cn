---
title: ICorProfilerCallback2::GarbageCollectionStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.GarbageCollectionStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::GarbageCollectionStarted
helpviewer_keywords:
- GarbageCollectionStarted method [.NET Framework profiling]
- ICorProfilerCallback2::GarbageCollectionStarted method [.NET Framework profiling]
ms.assetid: 44eef087-f21f-4fe2-b481-f8a0ee022e7d
topic_type:
- apiref
ms.openlocfilehash: 63a8d212a61bd73f44995f0e057eeff96f9a5554
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731938"
---
# <a name="icorprofilercallback2garbagecollectionstarted-method"></a>ICorProfilerCallback2::GarbageCollectionStarted 方法

通知代码探查器垃圾回收已启动。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GarbageCollectionStarted(  
    [in] int cGenerations,  
    [in, size_is(cGenerations), length_is(cGenerations)] BOOL generationCollected[],  
    [in] COR_PRF_GC_REASON reason);  
```  
  
## <a name="parameters"></a>参数  

 `cGenerations`  
 中数组中的总项数 `generationCollected` 。  
  
 `generationCollected`  
 中布尔值的数组， `true` 如果此垃圾回收正在收集与数组索引对应的生成，则为; 否则为 `false` 。  
  
 数组由 [COR_PRF_GC_GENERATION](cor-prf-gc-generation-enumeration.md) 枚举的一个值进行索引，该枚举指示代。  
  
 `reason`  
 中一个 [COR_PRF_GC_REASON](cor-prf-gc-reason-enumeration.md) 枚举的值，该值指示导致垃圾回收的原因。  
  
## <a name="remarks"></a>注解  

 与此垃圾回收相关的所有回调都将在 `GarbageCollectionStarted` 回调和相应的 [ICorProfilerCallback2：： GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) 回调之间发生。 这些回调不需要在同一线程上发生。  
  
 探查器在回调过程中检查其原始位置的对象是安全的 `GarbageCollectionStarted` 。 垃圾回收器将在返回后开始移动对象 `GarbageCollectionStarted` 。 在探查器从此回调返回后，探查器应在收到回调之前将所有对象 Id 视为无效 `ICorProfilerCallback2::GarbageCollectionFinished` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
