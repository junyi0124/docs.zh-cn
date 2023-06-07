---
title: ICorDebug::CanLaunchOrAttach 方法
ms.date: 03/30/2017
api_name:
- ICorDebug.CanLaunchOrAttach
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebug::CanLaunchOrAttach
helpviewer_keywords:
- ICorDebug::CanLaunchOrAttach method [.NET Framework debugging]
- CanLaunchOrAttach method [.NET Framework debugging]
ms.assetid: ca7723db-7c07-4cdd-bd92-fba34928b623
topic_type:
- apiref
ms.openlocfilehash: 195c7e1e7c61fd6ac8a21226b52e3782d2f7e421
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723488"
---
# <a name="icordebugcanlaunchorattach-method"></a>ICorDebug::CanLaunchOrAttach 方法

返回一个 HRESULT，它指示是否可以在当前计算机和运行时配置的上下文中启动新进程或附加到指定的现有进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CanLaunchOrAttach (  
    [in] DWORD      dwProcessId,  
    [in] BOOL       win32DebuggingEnabled  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwProcessId`  
 中现有进程的 ID。  
  
 `win32DebuggingEnabled`  
 中 `true` 如果计划在启用 win32 调试的情况下启动或附加到已启用 win32 调试，请 `false` 传入  
  
## <a name="return-value"></a>返回值  

 在给定有关当前计算机和运行时配置的信息的情况下，如果调试服务确定启动新进程或附加到给定进程，则 S_OK。 可能的 HRESULT 值为：  
  
- S_OK  
  
- CORDBG_E_DEBUGGING_NOT_POSSIBLE  
  
- CORDBG_E_KERNEL_DEBUGGER_PRESENT  
  
- CORDBG_E_KERNEL_DEBUGGER_ENABLED  
  
## <a name="remarks"></a>注解  

 此方法纯信息性。 无论返回的值是什么，接口都不会阻止你启动或附加到进程 `CanLaunchOrAttach` 。  
  
 如果计划在启用 Win32 调试的情况下启动或附加启用了 Win32 调试，请传递 `true` for `win32DebuggingEnabled` 。 如果使用此选项，则返回的 HRESULT `CanLaunchOrAttach` 可能不同。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebug 接口](icordebug-interface.md)
