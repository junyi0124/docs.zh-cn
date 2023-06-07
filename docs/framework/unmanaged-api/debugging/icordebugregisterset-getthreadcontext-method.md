---
title: ICorDebugRegisterSet::GetThreadContext 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet.GetThreadContext
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet::GetThreadContext
helpviewer_keywords:
- GetThreadContext method, ICorDebugRegisterSet interface [.NET Framework debugging]
- ICorDebugRegisterSet::GetThreadContext method [.NET Framework debugging]
ms.assetid: 0f63400b-dc1c-48d6-b51a-75c3f7f28e03
topic_type:
- apiref
ms.openlocfilehash: a7d78daf74d3cc01c2313f092bce53950dbd7bfb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681218"
---
# <a name="icordebugregistersetgetthreadcontext-method"></a>ICorDebugRegisterSet::GetThreadContext 方法

获取当前线程的上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetThreadContext(  
    [in] ULONG32 contextSize,  
    [in, out, length_is(contextSize),  
        size_is(contextSize)] BYTE context[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `contextSize`  
 中数组的大小（以字节为单位） `context` 。  
  
 `context`  
 [in，out]为当前平台构成 Win32 结构的字节数组 `CONTEXT` 。  
  
## <a name="remarks"></a>注解  

 调试器应调用此函数，而不是 Win32 `GetThreadContext` 函数，因为该线程可能处于暂时更改其上下文的 "被劫持" 状态。 返回的数据是 `CONTEXT` 当前平台的 Win32 结构。  
  
 对于非叶帧，客户端应使用 [ICorDebugRegisterSet：： GetRegistersAvailable](icordebugregisterset-getregistersavailable-method.md)检查哪些寄存器是有效的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
- [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)
