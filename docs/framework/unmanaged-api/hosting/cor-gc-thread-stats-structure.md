---
title: COR_GC_THREAD_STATS 结构
ms.date: 03/30/2017
api_name:
- COR_GC_THREAD_STATS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COR_GC_THREAD_STATS
helpviewer_keywords:
- COR_GC_THREAD_STATS structure [.NET Framework hosting]
ms.assetid: 01f9a59b-7679-4d42-9ced-4a8981625c3d
topic_type:
- apiref
ms.openlocfilehash: 25a90965dc5466b7cf1a07140705424cf2ba4cd9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699230"
---
# <a name="cor_gc_thread_stats-structure"></a>COR_GC_THREAD_STATS 结构

包含与垃圾回收相关的每个线程的统计信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _COR_GC_THREAD_STATS {  
    ULONGLONG  PerThreadAllocation;
    ULONG      Flags;
} COR_GC_THREAD_STATS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`PerThreadAllocation`|与当前实例关联的线程上分配的内存字节数 `COR_GC_THREAD_STATS` 。 每次发生零代垃圾回收时，此数字会被清除为零。|  
|`Flags`|最近一次垃圾回收后提升为较高代的字节数。|  
  
## <a name="remarks"></a>注解  

 [ICLRTask：： GetMemStats](iclrtask-getmemstats-method.md) 采用类型为的输出参数 `COR_GC_THREAD_STATS` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost .idl  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载结构](hosting-structures.md)
- [IHostTask 接口](ihosttask-interface.md)
