---
title: ICorDebugVariableHome：： GetOffset 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetOffset
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetOffset
helpviewer_keywords:
- ICorDebugVariableHome::GetOffset method [.NET Framework debugging]
- GetOffset method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: f025c2e5-3f6c-4be8-9ffe-c8b214617dfe
topic_type:
- apiref
ms.openlocfilehash: c5d491b66e4ec64dffa4e19dabff876c9c473036
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711788"
---
# <a name="icordebugvariablehomegetoffset-method"></a>ICorDebugVariableHome：： GetOffset 方法

获取与基寄存器相对应的偏移量。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetOffset(  
    [out] LONG *pOffset  
);  
```  
  
## <a name="parameters"></a>参数  

 `pOffset`  
 弄基寄存器的偏移量。  
  
## <a name="return-value"></a>返回值  

 方法返回以下值：  
  
|Value|说明|  
|-----------|-----------------|  
|`S_OK`|变量在寄存器相对内存位置。|  
|`E_FAIL`|变量不在寄存器相对内存位置中。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
