---
title: ICorDebugGCReferenceEnum 接口
ms.date: 03/30/2017
api_name:
- ICorDebugGCReferenceEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugGCReferenceEnum
helpviewer_keywords:
- ICorDebugGCReferenceEnum interface [.NET Framework debugging]
ms.assetid: 5f3c91c9-c035-454f-96cc-011cab1ea06b
topic_type:
- apiref
ms.openlocfilehash: 12ce800cb83ef4f79710aa441b50be860526023c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728116"
---
# <a name="icordebuggcreferenceenum-interface"></a>ICorDebugGCReferenceEnum 接口

提供针对将进行垃圾回收的对象的枚举器。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Next 方法](icordebuggcreferenceenum-next-method.md)|获取指定数量的 [COR_GC_REFERENCE](cor-gc-reference-structure.md) 实例，这些实例包含有关将进行垃圾回收的对象的信息。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugGCReferenceEnum`接口实现 "ICorDebugEnum" 接口。  
  
 `ICorDebugGCReferenceEnum`实例通过调用[ICorDebugProcess5：： EnumerateGCReferences](icordebugprocess5-enumerategcreferences-method.md)方法填充[COR_GC_REFERENCE](cor-gc-reference-structure.md)的实例。 可以通过调用[ICorDebugGCReference：： Next](icordebuggcreferenceenum-next-method.md)方法来枚举[COR_GC_REFERENCE](cor-gc-reference-structure.md)的对象。  
  
 此方法填充的集合中的 [COR_GC_REFERENCE](cor-gc-reference-structure.md) 对象表示三种对象：  
  
- 所有托管堆栈中的对象。 这包括托管代码中的实时引用以及由公共语言运行时创建的对象。  
  
- 句柄表中的对象。 这包括 `HNDTYPE_STRONG` 模块中 (和 `HNDTYPE_REFCOUNT`) 和静态变量的强引用。  
  
- 终结器队列中的对象。 终结器队列根对象，直到终结器运行。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
