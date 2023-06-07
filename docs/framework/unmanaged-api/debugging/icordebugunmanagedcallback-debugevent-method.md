---
title: ICorDebugUnmanagedCallback::DebugEvent 方法
ms.date: 03/30/2017
api_name:
- ICorDebugUnmanagedCallback.DebugEvent
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugUnmanagedCallback::DebugEvent
helpviewer_keywords:
- DebugEvent method [.NET Framework debugging]
- ICorDebugUnmanagedCallback::DebugEvent method [.NET Framework debugging]
ms.assetid: be9cab04-65ec-44d5-a39a-f90709fdd043
topic_type:
- apiref
ms.openlocfilehash: 75341b1af034972c861b75f29a06eaa2c4e33c3a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703026"
---
# <a name="icordebugunmanagedcallbackdebugevent-method"></a>ICorDebugUnmanagedCallback::DebugEvent 方法

通知调试器已激发本机事件。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DebugEvent (  
    [in] LPDEBUG_EVENT  pDebugEvent,  
    [in] BOOL           fOutOfBand  
);  
```  
  
## <a name="parameters"></a>参数  

 `pDebugEvent`  
 中指向本机事件的指针。  
  
 `fOutOfBand`  
 [in] `true` 如果在非托管事件发生后无法与托管进程状态交互，则在调试器调用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md)之前，则为; 否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 如果正在调试的线程是 Win32 线程，请不要使用 Win32 调试接口的任何成员。 只能 `ICorDebugController::Continue` 在 Win32 线程上调用，并且只能在带外事件继续。  
  
 `DebugEvent`回调不遵循用于回调的标准规则。 调用时 `DebugEvent` ，该进程将处于原始的 OS 调试停止状态。 将不同步该进程。 必要时，它会自动进入 "已同步" 状态以满足有关托管代码的信息请求，这可能会导致其他嵌套 `DebugEvent` 回调。  
  
 在进程上调用 [ICorDebugProcess：： ClearCurrentException](icordebugprocess-clearcurrentexception-method.md) ，以忽略异常事件，然后继续此过程。 调用此方法会在 CONTINUE 请求上发送 DBG_CONTINUE，而不是 DBG_EXCEPTION_NOT_HANDLED，并自动清除带外断点和单步例外。 即使在要调试的应用程序出现停止和已有的带内事件已存在时，也可以随时发出带外事件。  
  
 在 .NET Framework 版本2.0 中，调试程序应立即继续越过带外断点事件。 调试器应使用 [ICorDebugProcess2：： SetUnmanagedBreakpoint](icordebugprocess2-setunmanagedbreakpoint-method.md) 和 [ICorDebugProcess2：： ClearUnmanagedBreakpoint](icordebugprocess2-clearunmanagedbreakpoint-method.md) 方法添加和移除断点。 这些方法将自动跳过任何带外断点。 因此，调度的唯一带外断点应该是已在指令流中的原始断点，如对 Win32 函数的调用 `DebugBreak` 。 请勿尝试使用 `ICorDebugProcess::ClearCurrentException` 、 [ICorDebugProcess：： GetThreadContext](icordebugprocess-getthreadcontext-method.md)、 [ICorDebugProcess：： SetThreadContext](icordebugprocess-setthreadcontext-method.md)或 [调试 API](index.md)的任何其他成员。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugUnmanagedCallback 接口](icordebugunmanagedcallback-interface.md)
