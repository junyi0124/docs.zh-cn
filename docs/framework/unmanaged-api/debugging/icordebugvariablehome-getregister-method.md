---
title: ICorDebugVariableHome：： GetRegister 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetRegister
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetRegister
helpviewer_keywords:
- ICorDebugVariableHome::GetRegister method [.NET Framework debugging]
- GetRegister method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: a5eecd7b-b04c-4266-bff2-7c8771d519a8
topic_type:
- apiref
ms.openlocfilehash: 7f912f4b13620b567f5aa097604e98112d85f02d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711749"
---
# <a name="icordebugvariablehomegetregister-method"></a>ICorDebugVariableHome：： GetRegister 方法

获取一个寄存器，其中包含位置类型为的变量 `VLT_REGISTER` ，以及位置类型为的变量的基寄存器 `VLT_REGISTER_RELATIVE` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRegister(  
    [out] CorDebugRegister *pRegister  
);  
```  
  
## <a name="parameters"></a>参数  

 `pRegister`  
 弄一个 CorDebugRegister 枚举值，该值指示注册位置类型为的变量 `VLT_REGISTER` ，并为位置类型为的变量指定基寄存器 `VLT_REGISTER_RELATIVE` 。  
  
## <a name="return-value"></a>返回值  

 方法返回以下值：  
  
|Value|说明|  
|-----------|-----------------|  
|`S_OK`|变量在由参数指示的寄存器中 `pRegister` 。|  
|`E_FAIL`|变量不在寄存器或寄存器相对位置中。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [VariableLocationType 枚举](variablelocationtype-enumeration.md)
- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
