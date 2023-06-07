---
title: ICorProfilerCallback2::RootReferences2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback2.RootReferences2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback2::RootReferences2
helpviewer_keywords:
- RootReferences2 method [.NET Framework profiling]
- ICorProfilerCallback2::RootReferences2 method [.NET Framework profiling]
ms.assetid: 55a2f907-d216-42eb-8f2f-e5d59c2eebd6
topic_type:
- apiref
ms.openlocfilehash: 9e53e7bcecd900bb6c71d0a822e9b63ff6726e58
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729507"
---
# <a name="icorprofilercallback2rootreferences2-method"></a>ICorProfilerCallback2::RootReferences2 方法

发生垃圾回收后，通知探查器有关根引用的信息。 此方法是 [ICorProfilerCallback：： RootReferences](icorprofilercallback-rootreferences-method.md) 方法的扩展。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RootReferences2(  
    [in] ULONG  cRootRefs,  
    [in, size_is(cRootRefs)] ObjectID rootRefIds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_KIND rootKinds[],  
    [in, size_is(cRootRefs)] COR_PRF_GC_ROOT_FLAGS rootFlags[],  
    [in, size_is(cRootRefs)] UINT_PTR rootIds[]);  
```  
  
## <a name="parameters"></a>参数  

 `cRootRefs`  
 中 `rootRefIds`、 `rootKinds` 、 `rootFlags` 和数组中的元素数 `rootIds` 。  
  
 `rootRefIds`  
 中对象 Id 的数组，其中每个对象都引用堆栈上的静态对象或对象。 数组中的元素 `rootKinds` 提供用于对数组中的相应元素进行分类的信息 `rootRefIds` 。  
  
 `rootKinds`  
 中一个 [COR_PRF_GC_ROOT_KIND](cor-prf-gc-root-kind-enumeration.md) 值的数组，这些值指示垃圾回收根的类型。  
  
 `rootFlags`  
 中描述垃圾回收根属性的 [COR_PRF_GC_ROOT_FLAGS](cor-prf-gc-root-flags-enumeration.md) 值的数组。  
  
 `rootIds`  
 中一个 UINT_PTR 值的数组，这些值指向一个整数，该整数包含有关垃圾回收根的其他信息，具体取决于 `rootKinds` 参数的值。  
  
 如果根的类型为堆栈，则根 ID 适用于包含变量的函数。 如果该根 ID 为0，则该函数是 CLR 内部的一个未命名函数。 如果根的类型为句柄，则根 ID 用于垃圾回收句柄。 对于其他根类型，该 ID 是不透明值，应忽略它。  
  
## <a name="remarks"></a>注解  

 `rootRefIds`、 `rootKinds` 、 `rootFlags` 和 `rootIds` 数组是并行数组。 也就是说，、 `rootRefIds[i]` 、 `rootKinds[i]` `rootFlags[i]` 和 `rootIds[i]` 都涉及到相同的根。  
  
 同时 `RootReferences` `RootReferences2` 调用和来通知探查器。 探查器通常会实现一种方法或另一种方法，但不能同时实现两者，因为传入的信息 `RootReferences2` 是传入的的超集 `RootReferences` 。  
  
 中的项可以为 `rootRefIds` 零，这意味着对应的根引用为 null，并且不引用托管堆上的对象。  
  
 返回的对象 Id 在 `RootReferences2` 回调过程中无效，因为垃圾回收可能正处于将对象从旧地址移到新地址的过程中。 因此，探查器不应在 `RootReferences2` 调用期间尝试检查对象。 调用 [ICorProfilerCallback2：： GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) 时，所有对象已移动到它们的新位置，并且可以安全检查。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
