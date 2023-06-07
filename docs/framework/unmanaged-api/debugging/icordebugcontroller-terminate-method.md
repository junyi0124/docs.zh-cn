---
title: ICorDebugController::Terminate 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.Terminate
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::Terminate
helpviewer_keywords:
- Terminate method, ICorDebugController interface [.NET Framework debugging]
- ICorDebugController::Terminate method [.NET Framework debugging]
ms.assetid: 4275af0c-b5a7-4e4c-97c9-7e41f36b2dd8
topic_type:
- apiref
ms.openlocfilehash: 460aeeca9d62ce91a11a24d774c8e681ed4f00ca
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679801"
---
# <a name="icordebugcontrollerterminate-method"></a>ICorDebugController::Terminate 方法

用指定的退出代码终止进程。  
  
> [!NOTE]
> 此方法是 Win32 函数的包装器 `TerminateProcess` 。 因此， `Terminate` 使用退出代码的方式与 Win32 函数使用的方式相同 `TerminateProcess` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Terminate (  
    [in] UINT exitCode  
);  
```  
  
## <a name="parameters"></a>参数  

 `exitCode`  
 中一个表示退出代码的数字值。 有效的数值是在 Winbase.h 中定义的。  
  
## <a name="remarks"></a>注解  

 如果在调用时停止该进程 `Terminate` ，则应使用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md) 方法继续此过程，以便调试器通过 [ICorDebugManagedCallback：： ExitProcess](icordebugmanagedcallback-exitprocess-method.md) 或 [ICorDebugManagedCallback：： ExitAppDomain](icordebugmanagedcallback-exitappdomain-method.md) 回调接收终止确认。  
  
> [!NOTE]
> 此方法不是由应用程序域实现的。 也就是说，它不是在级别实现的 <xref:System.AppDomain> 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
