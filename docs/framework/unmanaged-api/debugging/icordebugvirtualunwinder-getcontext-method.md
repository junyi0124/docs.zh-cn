---
title: ICorDebugVirtualUnwinder::GetContext 方法
ms.date: 03/30/2017
ms.assetid: fe502a76-3068-47e5-a0a0-85ccb72dfac3
ms.openlocfilehash: a5a1afa47e52eff7c930698a3354a03d8c62259f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679450"
---
# <a name="icordebugvirtualunwindergetcontext-method"></a>ICorDebugVirtualUnwinder::GetContext 方法

获取此展开器的当前上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetContext(  
   [in] ULONG32 contextFlags,  
   [in] ULONG32 cbContextBuf,  
   [out] ULONG32* contextSize,  
   [out, size_is(cbContextBuf)] BYTE contextBuf[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `contextFlags`  
 [in] 指定要返回上下文的哪些部分的标记（在 WinNT.h 中定义）。  
  
 `cbContextBuf`  
 [in] `contextBuf` 中的字节数。  
  
 `contextSize`  
 [out] 指向实际写入 `contextBuf` 的字节数的指针。  
  
 `contextBuf`  
 [out] 包含此开卷机当前上下文的字节数组。  
  
## <a name="return-value"></a>返回值  

 由 mscordbi 接收到的任何失败的 HRESULT 都被视为是致命的，并将导致 ICorDebug API 返回 `CORDBG_E_DATA_TARGET_ERROR`。  
  
## <a name="remarks"></a>注解  

 将自变量的初始值设置 `contextBuf` 为通过调用 [ICorDebugStackWalk：： GetContext](icordebugstackwalk-getcontext-method.md) 方法返回的上下文缓冲区。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
 因为回退可能仅还原寄存器子集（例如非易失性寄存器），所以在实际方法调用时，上下文可能并不与寄存器状态完全匹配。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMemoryBuffer 接口](icordebugmemorybuffer-interface.md)
- [调试接口](debugging-interfaces.md)
