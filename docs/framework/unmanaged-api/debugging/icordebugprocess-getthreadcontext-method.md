---
title: ICorDebugProcess::GetThreadContext 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.GetThreadContext
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::GetThreadContext
helpviewer_keywords:
- ICorDebugProcess::GetThreadContext method [.NET Framework debugging]
- GetThreadContext method, ICorDebugProcess interface [.NET Framework debugging]
ms.assetid: 5b132ef1-8d4b-4525-89b3-54123596c194
topic_type:
- apiref
ms.openlocfilehash: 3be689e5c1474bcbfbca72a14a298762dc2e7a90
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724541"
---
# <a name="icordebugprocessgetthreadcontext-method"></a>ICorDebugProcess::GetThreadContext 方法

获取此进程中给定线程的上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetThreadContext(  
    [in] DWORD threadID,  
    [in] ULONG32 contextSize,  
    [in, out, length_is(contextSize), size_is(contextSize)]  
    BYTE context[]);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 中要为其检索上下文的线程的 ID。  
  
 `contextSize`  
 [in] `context` 数组的大小。  
  
 `context`  
 [in，out]用于描述线程上下文的字节数组。  
  
 上下文指定正在执行线程的处理器的体系结构。  
  
## <a name="remarks"></a>注解  

 调试器应调用此方法而不是 Win32 `GetThreadContext` 方法，因为该线程实际可能处于 "被劫持" 状态，在该状态下，其上下文已暂时更改。 仅当线程在本机代码中时，才应使用此方法。 在托管代码中对线程使用 [ICorDebugRegisterSet](icordebugregisterset-interface.md) 。  
  
 返回的数据是当前平台的上下文结构。 与 Win32 `GetThreadContext` 方法一样，调用方应在 `context` 调用此方法之前初始化参数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
