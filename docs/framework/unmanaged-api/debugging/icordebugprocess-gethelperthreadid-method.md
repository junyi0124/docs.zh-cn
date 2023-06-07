---
title: ICorDebugProcess::GetHelperThreadID 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.GetHelperThreadID
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::GetHelperThreadID
helpviewer_keywords:
- GetHelperThreadID method [.NET Framework debugging]
- ICorDebugProcess::GetHelperThreadID method [.NET Framework debugging]
ms.assetid: 84e1e605-37c1-49a5-8e12-35db85654622
topic_type:
- apiref
ms.openlocfilehash: 77cc658e28c7a69d8c4aeeed2f3e7ea40f0d2af6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724567"
---
# <a name="icordebugprocessgethelperthreadid-method"></a>ICorDebugProcess::GetHelperThreadID 方法

获取调试器的内部帮助器线程 (操作系统) 线程 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetHelperThreadID (  
    [out] DWORD *pThreadID  
);  
```  
  
## <a name="parameters"></a>参数  

 `pThreadID`  
 弄指向调试器的内部帮助器线程的操作系统线程 ID 的指针。  
  
## <a name="remarks"></a>注解  

 在托管和非托管调试期间，调试器负责确保具有指定 ID 的线程在命中调试器所放置的断点时保持运行状态。 调试器也可能希望从用户隐藏此线程。 如果进程中尚不存在 helper 线程，则该 `GetHelperThreadID` 方法将在 * 中返回零 `pThreadID` 。  
  
 不能缓存帮助器线程的线程 ID，因为它可能会随时间而改变。 必须在每次停止事件时重新查询线程 ID。  
  
 在每个非托管 [ICorDebugManagedCallback：： CreateThread](icordebugmanagedcallback-createthread-method.md) 事件上，调试器的帮助器线程的线程 id 都是正确的，从而允许调试器确定其帮助器线程的线程 id 并将其隐藏给用户。 在非托管事件期间识别为帮助器线程的线程 `ICorDebugManagedCallback::CreateThread` 永远不会运行托管的用户代码。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cordebug.idl。 Cordebug.idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
