---
title: ICorDebugManagedCallback::LogSwitch 方法
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback.LogSwitch
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback::LogSwitch
helpviewer_keywords:
- LogSwitch method [.NET Framework debugging]
- ICorDebugManagedCallback::LogSwitch method [.NET Framework debugging]
ms.assetid: 0ac59d27-783f-4a87-b7a8-baa3ccc54582
topic_type:
- apiref
ms.openlocfilehash: db6ccd63bbeadd7dcff1c7f8491b59017d431d12
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720693"
---
# <a name="icordebugmanagedcallbacklogswitch-method"></a>ICorDebugManagedCallback::LogSwitch 方法

通知调试器公共语言运行时 (CLR) 托管线程在类中调用了方法 <xref:System.Diagnostics.Switch> 来创建、修改或删除调试/跟踪开关。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT LogSwitch (  
    [in] ICorDebugAppDomain  *pAppDomain,  
    [in] ICorDebugThread     *pThread,  
    [in] LONG                 lLevel,  
    [in] ULONG                ulReason,  
    [in] WCHAR               *pLogSwitchName,  
    [in] WCHAR               *pParentName);  
```  
  
## <a name="parameters"></a>参数  

 `PAppDomain`  
 中指向 ICorDebugAppDomain 对象的指针，该对象表示包含创建、修改或删除调试/跟踪开关的托管线程的应用程序域。  
  
 `pThread`  
 中指向 ICorDebugThread 对象的指针，该对象表示托管线程。  
  
 `lLevel`  
 中一个值，该值指示写入事件日志的描述性消息的严重性级别。  
  
 `ulReason`  
 中 [LogSwitchCallReason](logswitchcallreason-enumeration.md) 枚举的值，该值指示在调试/跟踪开关上执行的操作。  
  
 `pLogSwitchName`  
 中指向调试/跟踪开关的名称的指针。  
  
 `pParentName`  
 中一个指针，指向调试/跟踪开关的父项的名称。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)
