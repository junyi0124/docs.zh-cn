---
title: ICorDebugHandleValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugHandleValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHandleValue
helpviewer_keywords:
- ICorDebugHandleValue interface [.NET Framework debugging]
ms.assetid: 66fcd2b8-ac66-414b-83a8-75a925e17772
topic_type:
- apiref
ms.openlocfilehash: e695a93036e00e651ecababb0e1407661bcc48d8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729078"
---
# <a name="icordebughandlevalue-interface"></a>ICorDebugHandleValue 接口

ICorDebugReferenceValue 的子类，表示调试器已为其创建了垃圾回收句柄的引用值。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Dispose 方法](icordebughandlevalue-dispose-method.md)|释放此对象所引用的句柄， `ICorDebugHandleValue` 而不显式释放接口指针。|  
|[GetHandleType 方法](icordebughandlevalue-gethandletype-method.md)|获取一个 CorDebugHandleType 值，该值描述此所引用的句柄的类型 `ICorDebugHandleValue` 。|  
  
## <a name="remarks"></a>注解  

 当 `ICorDebugReferenceValue` 执行调试的代码时，对象会失效。 `ICorDebugHandleValue`通过中断和继续来维护其引用，直到显式释放它。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
