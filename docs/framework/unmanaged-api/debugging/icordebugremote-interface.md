---
title: ICorDebugRemote 接口
ms.date: 03/30/2017
api_name:
- ICorDebugRemote
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugRemote
helpviewer_keywords:
- ICorDebugRemote interface [.NET Framework debugging]
ms.assetid: 53d073c6-fa02-40d2-82e1-b9452bb6abaa
topic_type:
- apiref
ms.openlocfilehash: 276d36c511105087190cb7e9dfeaa6932efc67ff
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712100"
---
# <a name="icordebugremote-interface"></a>ICorDebugRemote 接口

提供启动托管调试器或将其附加到远程目标进程的能力。  
  
## <a name="syntax"></a>语法  
  
```cpp  
interface ICorDebugRemote : IUnknown  
{  
    HRESULT CreateProcessEx  
      (  
      [in] ICorDebugRemoteTarget *     pRemoteTarget,  
      [in] LPCWSTR                     lpApplicationName,  
      [in] LPWSTR                      lpCommandLine,  
      [in] LPSECURITY_ATTRIBUTES       lpProcessAttributes,  
      [in] LPSECURITY_ATTRIBUTES       lpThreadAttributes,  
      [in] BOOL                        bInheritHandles,  
      [in] DWORD                       dwCreationFlags,  
      [in] PVOID                       lpEnvironment,  
      [in] LPCWSTR                     lpCurrentDirectory,  
      [in] LPSTARTUPINFOW              lpStartupInfo,  
      [in] LPPROCESS_INFORMATION       lpProcessInformation,  
      [in] CorDebugCreateProcessFlags  debuggingFlags,  
      [out] ICorDebugProcess **        ppProcess  
      );  
  
    HRESULT DebugActiveProcessEx  
      (  
      [in] ICorDebugRemoteTarget *   pRemoteTarget,  
      [in] DWORD                     dwProcessId,  
      [in] BOOL                      fWin32Attach,  
      [out] ICorDebugProcess **      ppProcess  
      );  
};  
```  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ICorDebugRemote::CreateProcessEx 方法](icordebugremote-createprocessex-method.md)|在远程计算机上创建用于托管调试的进程。|  
|[ICorDebugRemote::DebugActiveProcessEx 方法](icordebugremote-debugactiveprocessex-method.md)|在调试器下的远程计算机上启动进程。|  
  
## <a name="remarks"></a>注解  

 目前，此功能仅用于调试在远程 Macintosh 计算机上运行的基于 Silverlight 的应用程序目标。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)
- [ICorDebug 接口](icordebug-interface.md)

- [调试接口](debugging-interfaces.md)
