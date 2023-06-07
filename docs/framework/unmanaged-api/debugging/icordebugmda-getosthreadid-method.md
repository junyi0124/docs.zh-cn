---
title: ICorDebugMDA::GetOSThreadId 方法
ms.date: 03/30/2017
api_name:
- ICorDebugMDA.GetOSThreadId
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugMDA::GetOSThreadId
helpviewer_keywords:
- ICorDebugMDA::GetOSThreadId method [.NET Framework debugging]
- GetOSThreadId method [.NET Framework debugging]
ms.assetid: 7ca7c364-ade4-4219-b434-9f6ae2359be6
topic_type:
- apiref
ms.openlocfilehash: 80248bba6d11b8af07aa0517cb41c8a4f783b5e0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710878"
---
# <a name="icordebugmdagetosthreadid-method"></a>ICorDebugMDA::GetOSThreadId 方法

获取操作系统 (操作系统) 线程标识符，由 [ICorDebugMDA](icordebugmda-interface.md) 表示的托管调试助手 (MDA) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetOSThreadId (  
    [out] DWORD    *pOsTid  
);  
```  
  
## <a name="parameters"></a>参数  

 `pOsTid`  
 弄指向 OS 线程标识符的指针。  
  
## <a name="remarks"></a>注解  

 使用 OS 线程而不是 ICorDebugThread 来实现以下情况：在本机线程上或在尚未输入托管代码的托管线程上触发 MDA。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMDA 接口](icordebugmda-interface.md)
- [使用托管调试助手诊断错误](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
