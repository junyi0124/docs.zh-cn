---
title: COR_GC_STATS 结构
ms.date: 03/30/2017
api_name:
- COR_GC_STATS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- COR_GC_STATS
helpviewer_keywords:
- COR_GC_STATS structure [.NET Framework hosting]
ms.assetid: 8d4ff73e-739b-40f6-9349-359fbc99c2f9
topic_type:
- apiref
ms.openlocfilehash: 53a70c53a06ac55a2dab7c646018d63189ee0b36
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726218"
---
# <a name="cor_gc_stats-structure"></a>COR_GC_STATS 结构

提供有关公共语言运行时 (CLR) 的垃圾回收机制的统计信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _COR_GC_STATS {  
    ULONG   Flags;
    SIZE_T  ExplicitGCCount;  
    SIZE_T  GenCollectionsTaken[3];  
    SIZE_T  CommittedKBytes;
    SIZE_T  ReservedKBytes;  
    SIZE_T  Gen0HeapSizeKBytes;  
    SIZE_T  Gen1HeapSizeKBytes;  
    SIZE_T  Gen2HeapSizeKBytes;  
    SIZE_T  LargeObjectHeapSizeKBytes;  
    SIZE_T  KBytesPromotedFromGen0;  
    SIZE_T  KBytesPromotedFromGen1;  
} COR_GC_STATS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`Flags`|指示应计算并返回的字段值。|  
|`ExplicitGCCount`|指示外部请求强制执行的垃圾回收数。|  
|`GenCollectionsTaken`|指示为每个代执行的垃圾回收数。|  
|`CommittedKBytes`|所有堆中提交的总千字节数。|  
|`ReservedKBytes`|所有堆中保留的总千字节数。|  
|`Gen0HeapSizeKBytes`|代零堆的大小（以 kb 为单位）。|  
|`Gen1HeapSizeKBytes`|第一代堆的大小（以 kb 为单位）。|  
|`Gen2HeapSizeKBytes`|第二代堆的大小（以 kb 为单位）。|  
|`LargeObjectHeapSizeKBytes`|大型对象堆的大小（以 kb 为单位）。|  
|`KBytesPromotedFromGen0`|从零代升级到第1代的对象的大小（以 kb 为单位）。|  
|`KBytesPromotedFromGen1`|从第一代升级到第二代的对象的大小（以 kb 为单位）。|  
  
## <a name="remarks"></a>注解  

 [ICLRGCManager：： GetStats](iclrgcmanager-getstats-method.md)方法要求将 `Flags` 结构的字段 `COR_GC_STATS` 设置为一个或多个[COR_GC_STAT_TYPES](cor-gc-stat-types-enumeration.md)枚举的值，以指定要设置的统计信息。  
  
 下表将此结构提供的统计信息映射到两个 [COR_GC_STAT_TYPES](cor-gc-stat-types-enumeration.md) 枚举值 `COR_GC_COUNTS` 和 `COR_GC_MEMORYUSAGE` 。  
  
|指定者 COR_GC_COUNTS|指定者 COR_GC_MEMORYUSAGE|  
|----------------------------------|---------------------------------------|  
|`ExplicitGCCount`<br /><br /> `GenCollectionsTaken`|`CommittedKBytes`<br /><br /> `ReservedKBytes`<br /><br /> `Gen0HeapSizeKBytes`<br /><br /> `Gen1HeapSizeKBytes`<br /><br /> `Gen2HeapSizeKBytes`<br /><br /> `LargeObjectHeapSizeKBytes`<br /><br /> `KBytesPromotedFromGen0`<br /><br /> `KBytesPromotedFromGen1`|  
  
 用法的示例如下所示：  
  
```cpp  
COR_GC_STATS GCStats;  
GCStats.Flags = COR_GC_COUNTS | COR_GC_MEMORYUSAGE;  
pCLRGCManager->GetStats(&GCStats);  
```  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost .idl  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载结构](hosting-structures.md)
- [自动内存管理](../../../standard/automatic-memory-management.md)
- [垃圾回收](../../../standard/garbage-collection/index.md)
