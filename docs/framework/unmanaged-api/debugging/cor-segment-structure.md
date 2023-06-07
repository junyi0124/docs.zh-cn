---
title: COR_SEGMENT 结构
ms.date: 03/30/2017
api_name:
- COR_SEGMENT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_SEGMENT
helpviewer_keywords:
- COR_SEGMENT structure [.NET Framework debugging]
ms.assetid: 93aeecb9-7fef-4545-8daf-f566dfc47084
topic_type:
- apiref
ms.openlocfilehash: 738e29fa15340c76b055b608140f3c3bfbd29611
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726348"
---
# <a name="cor_segment-structure"></a>COR_SEGMENT 结构

包含有关托管堆中的内存区域的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _COR_SEGMENT {  
    CORDB_ADDRESS start;
    CORDB_ADDRESS end;
    CorDebugGenerationTypes gen;
    ULONG heap;
} COR_SEGMENT;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`start`|内存区域的起始地址。|  
|`end`|内存区域的结束地址。|  
|`gen`|显示内存区域生成的 [CorDebugGenerationTypes](cordebuggenerationtypes-enumeration.md) 枚举成员。|  
|`heap`|内存区域驻留的堆数。 有关详细信息，请参阅备注部分。|  
  
## <a name="remarks"></a>注解  

 `COR_SEGMENTS` 结构表示托管堆中的内存区域。  `COR_SEGMENTS` 对象是 [ICorDebugHeapRegionEnum](icordebugheapsegmentenum-interface.md) 集合对象的成员，通过调用 [ICorDebugProcess5::EnumerateHeapRegions](icordebugprocess5-enumerateheapregions-method.md) 方法填充。  
  
 `heap` 字段是处理器编号，对应报告的堆。 对于工作站垃圾回收器，其值始终为零，因为工作站仅有一个垃圾回收堆。 对于服务器垃圾回收器，其值对应于堆附加到的处理器。 请注意，根据垃圾回收器的实现细节，垃圾回收堆可能多于或少于实际的处理器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
