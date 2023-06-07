---
title: ICorDebugManagedCallback2::ExceptionUnwind 方法
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback2.ExceptionUnwind
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback2::ExceptionUnwind
helpviewer_keywords:
- ICorDebugManagedCallback2::ExceptionUnwind method [.NET Framework debugging]
- ExceptionUnwind method [.NET Framework debugging]
ms.assetid: aaf5938d-179c-4eaa-8d35-8523a4fadded
topic_type:
- apiref
ms.openlocfilehash: a15391b63012fec3d0e6a0aa67540c3d2541944c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671312"
---
# <a name="icordebugmanagedcallback2exceptionunwind-method"></a>ICorDebugManagedCallback2::ExceptionUnwind 方法

在异常展开过程中提供状态通知。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ExceptionUnwind (  
    [in] ICorDebugAppDomain                  *pAppDomain,  
    [in] ICorDebugThread                     *pThread,  
    [in] CorDebugExceptionUnwindCallbackType  dwEventType,  
    [in] DWORD                                dwFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `pAppDomain`  
 中指向 ICorDebugAppDomain 对象的指针，该对象表示包含引发异常的线程的应用程序域。  
  
 `pThread`  
 中指向 ICorDebugThread 对象的指针，该对象表示引发异常的线程。  
  
 `dwEventType`  
 中CorDebugExceptionUnwindCallbackType 枚举的一个值，该值指定在展开阶段由回调发出信号的事件。  
  
 `dwFlags`  
 中 [CorDebugExceptionFlags](cordebugexceptionflags-enumeration.md) 枚举的一个值，该值指定有关异常的其他信息。  
  
## <a name="remarks"></a>注解  

 `ExceptionUnwind` 在异常处理过程的展开阶段的各个点调用。 `ExceptionUnwind` 在展开单个异常时，可以多次调用。  
  
 如果 `dwEventType` = DEBUG_EXCEPTION_INTERCEPTED，指令指针将位于线程的叶帧中，在 (之前的序列点，这可能是几个说明，然后) 导致该异常的指令。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugManagedCallback2 接口](icordebugmanagedcallback2-interface.md)
- [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)
