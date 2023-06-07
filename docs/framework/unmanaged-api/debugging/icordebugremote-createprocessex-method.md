---
title: ICorDebugRemote::CreateProcessEx 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRemote.CreateProcessEx
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugRemoteTarget::CreateProcessEx
helpviewer_keywords:
- CreateProcessEx method, ICorDebugRemote interface [.NET Framework debugging]
- ICorDebugRemote::CreateProcessEx method [.NET Framework debugging]
ms.assetid: 41af93c7-e448-4251-8d4d-413d38c635f2
topic_type:
- apiref
ms.openlocfilehash: 37bf800f27754d1bf80aece962b7cbb85b1cbedc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712178"
---
# <a name="icordebugremotecreateprocessex-method"></a>ICorDebugRemote::CreateProcessEx 方法

在调试器下的远程计算机上启动进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateProcessEx (  
    [in]  ICorDebugRemoteTarget*      pRemoteTarget,  
    [in]  LPCWSTR                     lpApplicationName,  
    [in]  LPWSTR                      lpCommandLine,  
    [in]  LPSECURITY_ATTRIBUTES       lpProcessAttributes,  
    [in]  LPSECURITY_ATTRIBUTES       lpThreadAttributes,  
    [in]  BOOL                        bInheritHandles,  
    [in]  DWORD                       dwCreationFlags,  
    [in]  PVOID                       lpEnvironment,  
    [in]  LPCWSTR                     lpCurrentDirectory,  
    [in]  LPSTARTUPINFOW              lpStartupInfo,  
    [in]  LPPROCESS_INFORMATION       lpProcessInformation,  
    [in]  CorDebugCreateProcessFlags  debuggingFlags,  
    [out] ICorDebugProcess**          ppProcess  
);  
```  
  
## <a name="parameters"></a>参数  

 `pRemoteTarget`  
 中指向 [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)的指针。 用于确定将在其上启动进程的远程计算机。  
  
 `lpApplicationName`  
 中指向以 null 结尾的字符串的指针，该字符串指定由已启动的进程执行的模块。 该模块在调用进程的安全上下文中执行。  
  
 `lpCommandLine`  
 中指向以 null 结尾的字符串的指针，该字符串指定由已启动的进程执行的命令行。  
  
 `lpProcessAttributes`  
 中没有用于远程调试。  
  
 `lpThreadAttributes`  
 中没有用于远程调试。  
  
 `bInheritHandles`  
 中没有用于远程调试。  
  
 `dwCreationFlags`  
 中没有用于远程调试。  
  
 `lpEnvironment`  
 中指向新进程的环境块的指针。  
  
 `lpCurrentDirectory`  
 中指向以 null 结尾的字符串的指针，该字符串指定进程的当前目录的完整路径。 如果此参数为 null，则新进程将与调用进程具有相同的当前驱动器和目录。  
  
 `lpStartupInfo`  
 中没有用于远程调试。  
  
 `lpProcessInformation`  
 中没有用于远程调试。  
  
 `debuggingFlags`  
 中没有用于远程调试。  
  
 `ppProcess`  
 弄指向表示进程的 "ICorDebugProcess Interface" 对象地址的指针。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功启动远程计算机上的进程，并返回 "ICorDebugProcess 接口" 进行调试。  
  
 E_FAIL（或其他 E_ 返回代码）  
 无法在远程计算机上启动进程，并返回 "ICorDebugProcess Interface" 进行调试。  
  
## <a name="remarks"></a>注解  

 Silverlight 中不支持混合模式调试。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cordebug.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRemote 接口](icordebugremote-interface.md)
- [ICorDebug 接口](icordebug-interface.md)

- [调试接口](debugging-interfaces.md)
