---
title: ICorDebugFunction2::SetJMCStatus 方法
ms.date: 03/30/2017
api_name:
- ICorDebugFunction2.SetJMCStatus
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFunction2::SetJMCStatus
helpviewer_keywords:
- ICorDebugFunction2::SetJMCStatus method [.NET Framework debugging]
- SetJMCStatus method, ICorDebugFunction2 interface [.NET Framework debugging]
ms.assetid: 22c27b01-2869-4214-b840-5921f7c874fc
topic_type:
- apiref
ms.openlocfilehash: 55f219b5b834f365b87440e69bfa7d2c4e519235
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696084"
---
# <a name="icordebugfunction2setjmcstatus-method"></a>ICorDebugFunction2::SetJMCStatus 方法

为仅我的代码步进标记此 ICorDebugFunction2 所表示的函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetJMCStatus (  
    [in] BOOL   bIsJustMyCode  
);  
```  
  
## <a name="parameters"></a>参数  

 `bIsJustMyCode`  
 中设置为将 `true` 函数标记为用户代码; 否则设置为 `false` 。  
  
## <a name="return-values"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|已成功标记函数。|  
|`CORDBG_E_FUNCTION_NOT_DEBUGGABLE`|函数无法标记为用户代码，因为无法对其进行调试。|  
  
## <a name="remarks"></a>注解  

 仅我的代码分档器将跳过非用户代码。 用户代码必须是可调试代码的子集。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
