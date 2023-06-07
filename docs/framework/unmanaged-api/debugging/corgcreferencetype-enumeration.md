---
title: CorGCReferenceType 枚举
ms.date: 03/30/2017
api_name:
- CorGCReferenceType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorGCReferenceType
helpviewer_keywords:
- CorGCReferenceType
ms.assetid: d9f16439-5a36-4474-8ffd-4f0b2c2bb686
topic_type:
- apiref
ms.openlocfilehash: e2903637faa11a3c0a62080cc6fafcf1fc668a56
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704989"
---
# <a name="corgcreferencetype-enumeration"></a>CorGCReferenceType 枚举

标识要进行垃圾回收的对象的源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    CorHandleStrong = 1,  
    CorHandleStrongPinning = 2,  
    CorHandleWeakShort = 4,  
    CorHandleWeakRefCount = 8,  
    CorHandleStrongRefCount = 32,  
    CorHandleStrongDependent = 64,  
    CorHandleStrongAsyncPinned = 128,  
    CorHandleStrongSizedByref = 256,  
  
    CorReferenceStack = 0x80000001,  
    CorReferenceFinalizer = 0x80000002,  
  
    CorHandleStrongOnly = 0x1E3,  
    CorHandleWeakOnly = 0xC,  
    CorHandleAll = 0x7FFFFFFF  
} CorGCReferenceType  
```  
  
## <a name="members"></a>成员  
  
|成员名称|说明|  
|-----------------|-----------------|  
|`CorHandleStrong`|来自对象句柄表的强引用的句柄。|  
|`CorHandleStrongPinning`|来自对象句柄表的固定强引用的句柄。|  
|`CorHandleWeakShort`|来自对象句柄表的弱引用的句柄。|  
|`CorHandleWeakRefCount`|对象句柄表中弱引用计数对象的句柄。|  
|`CorHandleStrongRefCount`|对象句柄表中的引用计数对象的句柄。|  
|`CorHandleStrongDependent`|来自对象句柄表的依赖对象的句柄。|  
|`CorHandleStrongAsyncPinned`|来自对象句柄表的异步固定对象。|  
|`CorHandleStrongSizedByref`|在垃圾回收时间保留所有对象和对象根的集体闭合的近似大小的强句柄。|  
|`CorReferenceStack`|托管堆栈中的引用。|  
|`CorReferenceFinalizer`|终结器队列中的引用。|  
|CorHandleStrongOnly|仅返回句柄表中的强引用。 此值仅由 [ICorDebugProcess5：： EnumerateHandles](icordebugprocess5-enumeratehandles-method.md) 方法使用。|  
|`CorHandleWeakOnly`|仅返回句柄表中的弱引用。 此值仅由 [ICorDebugProcess5：： EnumerateHandles](icordebugprocess5-enumeratehandles-method.md) 方法使用。|  
|`CorHandleAll`|返回句柄表中的所有引用。 此值仅由 [ICorDebugProcess5：： EnumerateHandles](icordebugprocess5-enumeratehandles-method.md) 方法使用。|  
  
## <a name="remarks"></a>注解  

 `CorGCReferenceType`枚举的使用方式如下：  
  
- 作为 `type` [COR_GC_REFERENCE](cor-gc-reference-structure.md) 结构的字段的值，它指示引用或句柄的源。  
  
- 作为 `types` [ICorDebugProcess5：： EnumerateHandles](icordebugprocess5-enumeratehandles-method.md) 方法的参数，它指定要包括在枚举中的句柄的类型。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
