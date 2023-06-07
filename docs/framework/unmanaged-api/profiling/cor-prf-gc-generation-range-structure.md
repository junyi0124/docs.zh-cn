---
title: COR_PRF_GC_GENERATION_RANGE 结构
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_GENERATION_RANGE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_GENERATION_RANGE
helpviewer_keywords:
- COR_PRF_GC_GENERATION_RANGE structure [.NET Framework profiling]
ms.assetid: e7e07273-8d10-4a68-807e-59634e3f8c5e
topic_type:
- apiref
ms.openlocfilehash: a0ee2c9ce38272caef4960bfe5949c11083c12dd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674926"
---
# <a name="cor_prf_gc_generation_range-structure"></a>COR_PRF_GC_GENERATION_RANGE 结构

描述一个正进行垃圾回收的内存范围（即块）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct COR_PRF_GC_GENERATION_RANGE {  
    COR_PRF_GC_GENERATION generation;  
    ObjectID rangeStart;  
    UINT_PTR rangeLength;  
    UINT_PTR rangeLengthReserved;  
} COR_PRF_GC_GENERATION_RANGE;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`generation`|一个 [COR_PRF_GC_GENERATION](cor-prf-gc-generation-enumeration.md) 枚举的值，该值指定内存块所属的代。|  
|`rangeStart`|对象的 ID，该对象指定内存块的起始位置。|  
|`rangeLength`|一个指向整数的指针，该整数指定内存块的已使用部分的大小， (即块中使用的内存量) 。|  
|`rangeLengthReserved`|指向一个整数的指针，该整数指定内存块的大小 (即，为块) 预留的内存量。|  
  
## <a name="remarks"></a>注解  

 `rangeLength`仅当[ICorProfilerInfo2：： GetGenerationBounds](icorprofilerinfo2-getgenerationbounds-method.md)或[ICorProfilerInfo2：： GetObjectGeneration](icorprofilerinfo2-getobjectgeneration-method.md)（这两个均使用该 `COR_PRF_GC_GENERATION_RANGE` 结构）从[ICorProfilerCallback2：： GarbageCollectionStarted](icorprofilercallback2-garbagecollectionstarted-method.md)或[ICorProfilerCallback2：： GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md)方法调用时，此值才是准确的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corprof.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析结构](profiling-structures.md)
