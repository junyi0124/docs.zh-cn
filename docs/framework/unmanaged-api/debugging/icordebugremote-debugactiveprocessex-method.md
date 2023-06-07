---
title: ICorDebugRemote::DebugActiveProcessEx 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRemote.DebugActiveProcessEx
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugRemoteTarget::DebugActiveProcessEx
helpviewer_keywords:
- ICorDebugRemote::DebugActiveProcessEx method [.NET Framework debugging]
- DebugActiveProcessEx method, ICorDebugRemote interface [.NET Framework debugging]
ms.assetid: b0df5c5d-9a2e-47bf-894c-6f8a9fe24a1f
topic_type:
- apiref
ms.openlocfilehash: c9847fd6122aa32c95aecd5643a62a6775ae38d3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712113"
---
# <a name="icordebugremotedebugactiveprocessex-method"></a>ICorDebugRemote::DebugActiveProcessEx 方法

在调试器下的远程计算机上启动进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DebugActiveProcessEx (  
    [in]  ICorDebugRemoteTarget *   pRemoteTarget,  
    [in]  DWORD                     dwProcessId,  
    [in]  BOOL                      fWin32Attach,  
    [out] ICorDebugProcess **       ppProcess  
);  
```  
  
## <a name="parameters"></a>参数  

 `pRemoteTarget`  
 中指向 [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)的指针。 此参数用于确定正在运行进程的计算机。  
  
 `id`  
 中调试器要附加到的进程的 ID。  
  
 `win32Attach`  
 [in] `true` 如果调试器应该表现为进程的 Win32 调试器并调度非托管回调，则为;否则为 `false` 。  
  
 `ppProcess`  
 弄一个指向 "ICorDebugProcess" 对象地址的指针，该对象表示调试器已附加到的进程。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功附加到远程计算机上的进程。  
  
 E_FAIL（或其他 E_ 返回代码）  
 无法附加到远程计算机上的进程。  
  
## <a name="remarks"></a>注解  

 Silverlight 中不支持混合模式调试。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRemote 接口](icordebugremote-interface.md)
- [ICorDebug 接口](icordebug-interface.md)

- [调试接口](debugging-interfaces.md)
