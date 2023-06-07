---
title: ICorDebugStackWalk::GetContext 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStackWalk.GetContext Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStackWalk::GetContext
helpviewer_keywords:
- GetContext method, ICorDebugStackWalk interface [.NET Framework debugging]
- ICorDebugStackWalk::GetContext method [.NET Framework debugging]
ms.assetid: 081d1c95-152b-4797-8552-18453eb7b14b
topic_type:
- apiref
ms.openlocfilehash: 927f2077b4bb71177c24816774d06643ebdaa922
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711957"
---
# <a name="icordebugstackwalkgetcontext-method"></a>ICorDebugStackWalk::GetContext 方法

返回 [ICorDebugStackWalk](icordebugstackwalk-interface.md) 对象中的当前帧的上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetContext([in]  ULONG32 contextFlags,  
                   [in]  ULONG32 contextBufSize,  
                   [out] ULONG32* contextSize,  
                   [out, size_is(contextBufSize)] BYTE contextBuf[]);  
```  
  
## <a name="parameters"></a>参数  

 `contextFlags`  
 中指示) 中定义的上下文缓冲区 (内容的标志。  
  
 `contextBufSize`  
 中上下文缓冲区的已分配大小。  
  
 `contextSize`  
 弄上下文的实际大小。 此值必须小于或等于上下文缓冲区的大小。  
  
 `contextBuf`  
 弄上下文缓冲区。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|当前帧的上下文已成功返回。|  
|E_FAIL|未能返回上下文。|  
|HRESULT_FROM_WIN32 (ERROR_INSUFFICIENT 缓冲区) |上下文缓冲区太小。|  
|CORDBG_E_PAST_END_OF_STACK|帧指针已位于堆栈末尾;因此，不能访问其他帧。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 因为展开仅还原一个寄存器子集，如非易失性寄存器，所以在调用时，上下文可能与注册状态不完全匹配。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
