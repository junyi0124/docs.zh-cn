---
title: ICorDebugExceptionObjectCallStackEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugExceptionObjectCallStackEnum::Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugExceptionObjectCallStackEnum::Next
helpviewer_keywords:
- ICorDebugExceptionObjectCallStackEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugExceptionObjectCallStackEnum interface [.NET Framework debugging]
ms.assetid: 3328a2c0-1e48-4a54-802a-9b474cf82c21
topic_type:
- apiref
ms.openlocfilehash: 17d5367564ec1ec98efc264ad9a5794c0d04a947
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672131"
---
# <a name="icordebugexceptionobjectcallstackenumnext-method"></a>ICorDebugExceptionObjectCallStackEnum::Next 方法

获取指定数量的 [CorDebugExceptionObjectStackFrame](cordebugexceptionobjectstackframe-structure.md) 实例，其中包含异常对象的调用堆栈中的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next(  
    [in] ULONG celt,  
    [out, size_is(celt), length_is(*pceltFetched)] CorDebugExceptionObjectStackFrame values[],  
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 中要检索的 [CorDebugExceptionObjectStackFrame](cordebugexceptionobjectstackframe-structure.md) 实例的数目。  
  
 `values`  
 弄指针的数组，其中每个指针指向一个 [CorDebugExceptionObjectStackFrame](cordebugexceptionobjectstackframe-structure.md) 对象。  
  
 `pceltFetched`  
 弄一个指针，指向实际返回的 [CorDebugExceptionObjectStackFrame](cordebugexceptionobjectstackframe-structure.md) 实例的数目。  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugExceptionObjectCallStackEnum 接口](icordebugexceptionobjectcallstackenum-interface.md)
- [调试接口](debugging-interfaces.md)
