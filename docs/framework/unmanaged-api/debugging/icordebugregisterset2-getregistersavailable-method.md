---
title: ICorDebugRegisterSet2::GetRegistersAvailable 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet2.GetRegistersAvailable
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet2::GetRegistersAvailable
helpviewer_keywords:
- GetRegistersAvailable method, ICorDebugRegisterSet2 interface [.NET Framework debugging]
- ICorDebugRegisterSet2::GetRegistersAvailable method [.NET Framework debugging]
ms.assetid: f3ed344b-0d3a-44e8-8000-2a97e0805a2c
topic_type:
- apiref
ms.openlocfilehash: cb56ea817d4045c19793a6290d68ae8b6236f14a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712308"
---
# <a name="icordebugregisterset2getregistersavailable-method"></a>ICorDebugRegisterSet2::GetRegistersAvailable 方法

获取提供可用寄存器的位图的字节数组。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRegistersAvailable (  
    [in] ULONG32 numChunks,  
    [out, size_is(numChunks)] BYTE availableRegChunks[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `numChunks`  
 [in] `availableRegChunks` 数组的大小。  
  
 `availableRegChunks`  
 弄字节数组，其中每个位对应于寄存器。 如果寄存器可用，则会设置寄存器的相应位。  
  
## <a name="remarks"></a>注解  

 CorDebugRegister 枚举的值指定不同微处理器的寄存器。 每个值的前五位是字节数组中的索引 `availableRegChunks` 。 每个值的下三位标识索引字节内的位位置。 给定一个 `CorDebugRegister` 指定特定寄存器的值时，将按如下所示确定此寄存器在掩码中的位置：  
  
1. 提取访问数组中正确字节所需的索引 `availableRegChunks` ：  
  
     `CorDebugRegister` 值 >> 3  
  
2. 提取索引字节内的位位置，其中位零是最小有效位：  
  
     `CorDebugRegister` 值 & 7  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)
- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
