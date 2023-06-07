---
title: ICorDebugRegisterSet2::GetRegisters 方法
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet2.GetRegisters
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet2::GetRegisters
helpviewer_keywords:
- GetRegisters method, ICorDebugRegisterSet2 interface [.NET Framework debugging]
- ICorDebugRegisterSet2::GetRegisters method [.NET Framework debugging]
ms.assetid: dbc498a8-ba3f-42f2-bdd9-b623c77a1019
topic_type:
- apiref
ms.openlocfilehash: 71b9d59621efb547713cb4a6c9df7a7142f4a677
ms.sourcegitcommit: 27db07ffb26f76912feefba7b884313547410db5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83615184"
---
# <a name="icordebugregisterset2getregisters-method"></a>ICorDebugRegisterSet2：： GetRegisters 方法

获取由给定位掩码指定的每个寄存器（对于当前正在执行其代码的平台）的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRegisters (  
    [in] ULONG32 maskCount,  
    [in, size_is(maskCount)] BYTE mask[],  
    [in] ULONG32 regCount,  
    [out, size_is(regCount)] CORDB_REGISTER regBuffer[]  
);  
```  
  
## <a name="parameters"></a>参数

 `maskCount`  
 中数组的大小（以字节为单位） `mask` 。  
  
 `mask`  
 中字节数组，其中每个位对应于寄存器。 如果位为1，则将检索相应寄存器的值。  
  
 `regCount`  
 中要检索的寄存器值的数目。  
  
 `regBuffer`  
 弄对象的数组 `CORDB_REGISTER` ，其中每个对象都接收寄存器的值。  
  
## <a name="remarks"></a>备注

 `GetRegisters`方法从掩码指定的寄存器返回值的数组。 数组不包含掩码位未设置的寄存器的值。 因此，数组的大小 `regBuffer` 必须等于掩码中1的数目。 如果的值 `regCount` 对于掩码指定的寄存器数量太小，则将从该集中截断编号较高的寄存器的值。 如果 `regCount` 太大，则不会修改未使用的 `regBuffer` 元素。  
  
 如果掩码指出某个不可用的寄存器，则将为该寄存器返回一个不确定的值。  
  
 `ICorDebugRegisterSet2::GetRegisters`对于具有超过64寄存器的平台，此方法是必需的。 例如，IA64 具有128常规用途寄存器和128浮点寄存器，因此位掩码中需要超过64位。  
  
 如果寄存器不超过64，则此 `GetRegisters` 方法只是将字节数组中的字节转换为， `mask` `ULONG64` 然后调用[ICorDebugRegisterSet：： GetRegisters](icordebugregisterset-getregisters-method.md)方法，该方法采用 `ULONG64` 掩码。  
  
## <a name="requirements"></a>要求

 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)
- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
