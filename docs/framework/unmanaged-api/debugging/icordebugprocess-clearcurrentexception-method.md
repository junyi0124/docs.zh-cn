---
title: ICorDebugProcess::ClearCurrentException 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.ClearCurrentException
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::ClearCurrentException
helpviewer_keywords:
- ClearCurrentException method, ICorDebugProcess interface [.NET Framework debugging]
- ICorDebugProcess::ClearCurrentException method [.NET Framework debugging]
ms.assetid: 9e02ee1a-e495-4578-bfb5-b946274bede7
topic_type:
- apiref
ms.openlocfilehash: 0d36390a905561b64b3ca6ca95722f82158450be
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695213"
---
# <a name="icordebugprocessclearcurrentexception-method"></a>ICorDebugProcess::ClearCurrentException 方法

清除给定线程上的当前非托管异常。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ClearCurrentException([in] DWORD threadID);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 中将清除当前非托管异常的线程的 ID。  
  
## <a name="remarks"></a>注解  

 在线程报告了应被调试对象忽略的非托管异常之前，请在调用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md) 之前调用此方法。 这会清除给定线程上未完成的带外 (IB) 和带外 (OOB) 事件。 自动清除所有 OOB 断点和单步例外。  
  
 使用 [ICorDebugThread2：： InterceptCurrentException](icordebugthread2-interceptcurrentexception-method.md) 来截获线程上的当前托管异常。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
