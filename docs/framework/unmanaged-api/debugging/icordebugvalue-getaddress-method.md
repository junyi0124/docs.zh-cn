---
title: ICorDebugValue::GetAddress 方法
ms.date: 03/30/2017
api_name:
- ICorDebugValue.GetAddress
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue::GetAddress
helpviewer_keywords:
- ICorDebugValue::GetAddress method [.NET Framework debugging]
- GetAddress method, ICorDebugValue interface [.NET Framework debugging]
ms.assetid: a247c792-45e1-4538-9e1f-b46acca4a463
topic_type:
- apiref
ms.openlocfilehash: 47c0c4dfa78e85bcc83f0bb2a333955c8e8666fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728363"
---
# <a name="icordebugvaluegetaddress-method"></a>ICorDebugValue::GetAddress 方法

获取此 "ICorDebugValue" 对象的地址，该对象处于调试过程中。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAddress (  
    [out] CORDB_ADDRESS   *pAddress  
);  
```  
  
## <a name="parameters"></a>参数  

 `pAddress`  
 弄指向 `CORDB_ADDRESS` 对象的指针，该对象指定此值对象的地址。  
  
## <a name="remarks"></a>注解  

 如果值不可用，则返回 0 (零) 。 如果值至少出现在寄存器中或者存储在垃圾回收器句柄 () 中，则可能会发生这种情况 `GCHandle` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
