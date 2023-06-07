---
title: ICorDebugVariableHome：： GetSlotIndex 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetSlotIndex
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetSlotIndex
helpviewer_keywords:
- ICorDebugVariableHome::GetSlotIndex method [.NET Framework debugging]
- GetSlotIndex method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: 966da50d-5665-4fff-bf7b-1c72bbadd9a4
topic_type:
- apiref
ms.openlocfilehash: 4b071bd8e9d96084848c1553385eec5f8beca624
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711723"
---
# <a name="icordebugvariablehomegetslotindex-method"></a>ICorDebugVariableHome：： GetSlotIndex 方法

获取本地变量的托管槽索引。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetSlotIndex(  
    [out] ULONG32 *pSlotIndex  
);  
```  
  
## <a name="parameters"></a>参数  

 `pSlotIndex`  
 弄指向局部变量的槽索引的指针。  
  
## <a name="return-value"></a>返回值  

 方法返回以下值。  
  
|Value|说明|  
|-----------|-----------------|  
|`S_OK`|方法调用返回中的槽索引值 `pSlotIndex` 。|  
|`E_FAIL`|当前 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 实例表示函数参数。|  
  
## <a name="remarks"></a>注解  

 槽索引可用于检索此局部变量的元数据。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
