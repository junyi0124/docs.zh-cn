---
title: ICorDebugRegisterSet::GetRegisters 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet.GetRegisters
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet::GetRegisters
helpviewer_keywords:
- GetRegisters method, ICorDebugRegisterSet interface [.NET Framework debugging]
- ICorDebugRegisterSet::GetRegisters method [.NET Framework debugging]
ms.assetid: fdf91864-48ea-4aa6-b70c-361b7a3184c7
topic_type:
- apiref
ms.openlocfilehash: 315e4cc3b93fc78e11a4fb399bbe6f8a9f55ac84
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705002"
---
# <a name="icordebugregistersetgetregisters-method"></a>ICorDebugRegisterSet::GetRegisters 方法

获取计算机上每个寄存器 (的值，该计算机当前正在执行位掩码指定的代码) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRegisters (  
    [in] ULONG64       mask,
    [in] ULONG32       regCount,  
    [out, size_is(regCount), length_is(regCount)]  
        CORDB_REGISTER regBuffer[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `mask`  
 中指定要检索的寄存器值的位掩码。 每个位对应于一个寄存器。 如果将位设置为1，则将检索寄存器的值;否则，不检索寄存器的值。  
  
 `regCount`  
 中要检索的寄存器值的数目。  
  
 `regBuffer`  
 弄对象的数组 `CORDB_REGISTER` ，其中每个对象都接收寄存器的值。  
  
## <a name="remarks"></a>注解  

 数组的大小应等于位掩码中设置为1的位数。 `regCount`参数指定将接收寄存器值的缓冲区中的元素数。 如果 `regCount` 该值对于掩码所指示的寄存器数量太小，则将从该集中截断编号较高的寄存器。 如果 `regCount` 值太大，则 `regBuffer` 不会修改未使用的元素。  
  
 如果位掩码指定的寄存器不可用，则 `GetRegisters` 返回该寄存器的不确定值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
- [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)
