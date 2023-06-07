---
title: ICorProfilerCallback::RootReferences 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RootReferences
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RootReferences
helpviewer_keywords:
- RootReferences method [.NET Framework profiling]
- ICorProfilerCallback::RootReferences method [.NET Framework profiling]
ms.assetid: dbdf853b-d1a4-4828-8ef7-53d121d8e6ae
topic_type:
- apiref
ms.openlocfilehash: 2d084ce0a785ba37c5b7dc937ed116cee74b7594
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720654"
---
# <a name="icorprofilercallbackrootreferences-method"></a>ICorProfilerCallback::RootReferences 方法

在垃圾回收后通知探查器有关根引用的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RootReferences(  
    [in] ULONG    cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[] );  
```  
  
## <a name="parameters"></a>参数  

 `cRootRefs`  
 中数组中的引用数 `rootRefIds` 。  
  
 `rootRefIds`  
 中对象 Id 的数组，这些对象 Id 引用静态对象或堆栈上的对象。  
  
## <a name="remarks"></a>注解  

 同时 `RootReferences` 调用和 [ICorProfilerCallback2：： RootReferences2](icorprofilercallback2-rootreferences2-method.md) 来通知探查器。 探查器通常会实现一个或另一个，但不能同时实现两者，因为传入的信息 `RootReferences2` 是传入的的 `RootReferences` 超集。  
  
 `rootRefIds`数组可以包含空对象。 例如，在堆栈上声明的所有对象引用都被垃圾回收器视为根，并将始终报告。  
  
 返回的对象 Id 在 `RootReferences` 回调过程中无效，因为垃圾回收可能正处于将对象从旧地址移到新地址的过程中。 因此，探查器不能尝试在调用过程中检查对象 `RootReferences` 。 调用 [ICorProfilerCallback2：： GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) 时，所有对象已移动到它们的新位置，并且可以安全检查。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
