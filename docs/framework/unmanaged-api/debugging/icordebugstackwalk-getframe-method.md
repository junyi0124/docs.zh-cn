---
title: ICorDebugStackWalk::GetFrame 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStackWalk.GetFrame Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStackWalk::GetFrame
helpviewer_keywords:
- GetFrame method [.NET Framework debugging]
- ICorDebugStackWalk::GetFrame method [.NET Framework debugging]
ms.assetid: 4083b505-5b59-44fb-8c5d-129db6a96c10
topic_type:
- apiref
ms.openlocfilehash: 452635764794e01858baab10464a03c966a55271
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711931"
---
# <a name="icordebugstackwalkgetframe-method"></a>ICorDebugStackWalk::GetFrame 方法

获取 [ICorDebugStackWalk](icordebugstackwalk-interface.md) 对象中的当前帧。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFrame([out] ICorDebugFrame ** pFrame);  
```  
  
## <a name="parameters"></a>参数  

 `pFrame`  
 中指向所创建的帧对象的地址的指针，该对象表示堆栈中的当前帧。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|运行时成功返回了当前帧。|  
|E_FAIL|当前帧未返回。|  
|S_FALSE|当前帧是本机堆栈帧。|  
|E_INVALIDARG|`pFrame` 为 null。|  
|CORDBG_E_PAST_END_OF_STACK|帧指针已位于堆栈末尾;因此，不能访问其他帧。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 `ICorDebugStackWalk` 仅返回实际的堆栈帧。 使用 [ICorDebugThread3：： GetActiveInternalFrames](icordebugthread3-getactiveinternalframes-method.md) 方法返回内部帧。  (内部帧是由运行时推送到堆栈上的数据结构，用于存储临时数据。 )   
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugStackWalk 接口](icordebugstackwalk-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
