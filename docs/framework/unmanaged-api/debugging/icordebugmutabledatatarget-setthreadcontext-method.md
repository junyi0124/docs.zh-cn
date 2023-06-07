---
title: ICorDebugMutableDataTarget::SetThreadContext 方法
ms.date: 03/30/2017
ms.assetid: 8c0d01d5-67e5-4522-9ccf-c8f3a78cb4fd
ms.openlocfilehash: 558a1ee2eb0ac06213c2ebcebbd5595b69ecc43e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695705"
---
# <a name="icordebugmutabledatatargetsetthreadcontext-method"></a>ICorDebugMutableDataTarget::SetThreadContext 方法

设置某个线程的上下文（寄存器值）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetThreadContext(  
   [in] DWORD dwThreadID,  
   [in] ULONG32 contextSize,   [in, size_is(contextSize)] const BYTE * pContext);  
```  
  
## <a name="parameters"></a>参数  

 `dwThreadID`  
 [in] 由操作系统定义的线程标识符。  
  
 `contextSize`  
 [in] 要写入的 `pContext` 缓冲区的大小。  
  
 `pContext`  
 [in] 指向要写入的字节的指针。  
  
## <a name="remarks"></a>注解  

 `SetThreadContext` 方法将更新由操作系统定义的 `dwThreadID` 参数指定的当前线程上下文。 上下文记录的格式由 [ICorDebugDataTarget：： GetPlatform](icordebugdatatarget-getplatform-method.md) 方法指示的平台决定。 在 Windows 上，这是一个 [上下文](/windows/win32/api/winnt/ns-winnt-arm64_nt_context) 结构。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMutableDataTarget 接口](icordebugmutabledatatarget-interface.md)
- [调试接口](debugging-interfaces.md)
