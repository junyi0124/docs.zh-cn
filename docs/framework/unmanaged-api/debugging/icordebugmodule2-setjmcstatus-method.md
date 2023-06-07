---
title: ICorDebugModule2::SetJMCStatus 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule2.SetJMCStatus
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule2::SetJMCStatus
helpviewer_keywords:
- SetJMCStatus method, ICorDebugModule2 interface [.NET Framework debugging]
- ICorDebugModule2::SetJMCStatus method [.NET Framework debugging]
ms.assetid: 8c6d2089-4dbb-4715-b9e9-2a4491c8c9ce
topic_type:
- apiref
ms.openlocfilehash: cfa6df7a812559f05a4c57381a5007c9c90238e1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709643"
---
# <a name="icordebugmodule2setjmcstatus-method"></a>ICorDebugModule2::SetJMCStatus 方法

将此 ICorDebugModule2 中所有类的所有方法的仅我的代码 (JMC) 状态设置为指定的值，数组中的所有方法的状态将 `pTokens` 设置为相反值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetJMCStatus (  
    [in] BOOL                        bIsJustMyCode,  
    [in] ULONG32                     cTokens,  
    [in, size_is(cTokens)] mdToken   pTokens[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `bIsJustMycode`  
 中 `true` 如果要调试代码，则设置为; 否则设置为 `false` 。  
  
 `cTokens`  
 [in] `pTokens` 数组的大小。  
  
 `pTokens`  
 中值的数组 `mdToken` ，其中每个值都是一个将其 JMC 状态设置为！的方法 `bIsJustMycode` 。  
  
## <a name="remarks"></a>注解  

 数组中指定的每个方法的 JMC 状态 `pTokens` 设置为与值相反的状态 `bIsJustMycode` 。 此模块中所有其他方法的状态将设置为 `bIsJustMycode` 值。  
  
 `SetJMCStatus`方法会清除此模块中的所有以前的 JMC 设置。  
  
 `SetJMCStatus`如果已成功设置所有函数，则方法将返回 S_OK HRESULT。 如果标记的某些函数不可调试，它将返回 CORDBG_E_FUNCTION_NOT_DEBUGGABLE HRESULT `true` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
