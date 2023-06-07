---
title: ICorDebugRegisterSet::GetRegistersAvailable 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet.GetRegistersAvailable
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet::GetRegistersAvailable
helpviewer_keywords:
- ICorDebugRegisterSet::GetRegistersAvailable method [.NET Framework debugging]
- GetRegistersAvailable method, ICorDebugRegisterSet interface [.NET Framework debugging]
ms.assetid: 4ba08ffa-55a2-4662-9d6d-4738f1db60c9
topic_type:
- apiref
ms.openlocfilehash: d28c130e55cbebf29348752780c03b03c1b8f358
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716988"
---
# <a name="icordebugregistersetgetregistersavailable-method"></a>ICorDebugRegisterSet::GetRegistersAvailable 方法

获取指示此 [ICorDebugRegisterSet](icordebugregisterset-interface.md) 中的哪些寄存器当前可用的位掩码。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRegistersAvailable (  
    [out] ULONG64   *pAvailable  
);  
```  
  
## <a name="parameters"></a>参数  

 `pAvailable`  
 弄一个位掩码，用于指示当前可用的注册。  
  
## <a name="remarks"></a>注解  

 如果无法为给定情况确定其值，则寄存器可能不可用。  
  
 对于每个寄存器 (1 << 注册索引) ，返回的掩码包含一个位。 如果寄存器可用，则位值为 1; 否则为0。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
- [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)
