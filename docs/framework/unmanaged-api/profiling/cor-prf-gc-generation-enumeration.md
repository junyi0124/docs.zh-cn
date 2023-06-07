---
title: COR_PRF_GC_GENERATION 枚举
ms.date: 03/30/2017
api_name:
- COR_PRF_GC_GENERATION
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_GC_GENERATION
helpviewer_keywords:
- COR_GC_GENERATION_FLAGS enumeration [.NET Framework profiling]
ms.assetid: d6ece160-26ad-4d39-abd7-05acd6f78c48
topic_type:
- apiref
ms.openlocfilehash: 1d7489e997868a9486f77d176580cee18213a99d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718536"
---
# <a name="cor_prf_gc_generation-enumeration"></a>COR_PRF_GC_GENERATION 枚举

标识垃圾回收生成。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    COR_PRF_GC_GEN_0 = 0,  
    COR_PRF_GC_GEN_1 = 1,  
    COR_PRF_GC_GEN_2 = 2,  
    COR_PRF_GC_LARGE_OBJECT_HEAP = 3,
    COR_PRF_GC_PINNED_OBJECT_HEAP= 4
} COR_PRF_GC_GENERATION;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`COR_PRF_GC_GEN_0`|对象存储为第0代。|  
|`COR_PRF_GC_GEN_1`|对象存储为第1代。|  
|`COR_PRF_GC_GEN_2`|对象存储为第2代。|  
|`COR_PRF_GC_LARGE_OBJECT_HEAP`|对象存储在大型对象堆中。|  
|`COR_PRF_GC_PINNED_OBJECT_HEAP`|对象存储在固定对象堆中。|  
  
## <a name="remarks"></a>注解  

 垃圾回收器通过将对象划分为基于年龄的代来改善内存管理性能。 垃圾回收器当前使用三个代，编号为0、1和2，以及两个特殊堆段，一个用于大型对象，一个用于固定的对象。
  
 大小大于阈值的对象存储在大对象堆中。 固定的对象可以分配给固定对象堆，以避免在正常堆上分配这些对象的性能成本。 其他已分配对象开始属于0代。 发生垃圾回收后存在的所有对象都将升级到第1代。 在第1代中发生垃圾回收后存在的对象将移入第2代。  
  
 代的使用意味着垃圾回收器在任一时间只需要处理已分配对象的子集。  
  
 `COR_PRF_GC_GENERATION`枚举由[COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md)结构使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
