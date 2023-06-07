---
title: ICorDebugCode3::GetReturnValueLiveOffset 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorDebugCode3.GetReturnValueLiveOffset
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode3::GetReturnValueLiveOffset
helpviewer_keywords:
- ICorDebugCode3::GetReturnValueLiveOffset method [.NET Framework debugging]
- GetReturnValueLiveOffset method [.NET Framework debugging]
ms.assetid: 8c2ff5d8-8c04-4423-b1e1-e1c8764b36d3
topic_type:
- apiref
ms.openlocfilehash: 6153ebf24ae939a50d71cad2d4323090aa905851
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720810"
---
# <a name="icordebugcode3getreturnvalueliveoffset-method"></a>ICorDebugCode3::GetReturnValueLiveOffset 方法

对于指定的 IL 偏移量，获取应放置断点的本机偏移量，以便调试器可以从函数中获取返回值。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetReturnValueLiveOffset(  
    [in] ULONG32 ILoffset,  
    [in] ULONG32 bufferSize,
    [out] ULONG32 *pFetched,
    [out, size_is(buffersize), length_is(*pFetched)] ULong32 pOffsets[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `ILoffset`  
 IL 偏移量。 它必须是函数调用站点，否则函数调用将失败。  
  
 `bufferSize`  
 可用于存储的字节数 `pOffsets` 。  
  
 `pFetched`  
 指向实际返回的偏移量的指针。 通常，其值为1，但单个 IL 指令可以映射到多个 `CALL` 程序集指令。  
  
 `pOffsets`  
 本机偏移量的数组。 通常， `pOffsets` 包含单个偏移量，但单个 IL 指令可以映射到多个 `CALL` 程序集指令。  
  
## <a name="remarks"></a>注解  

 此方法与 [ICorDebugILFrame3：： GetReturnValueForILOffset](icordebugilframe3-getreturnvalueforiloffset-method.md) 方法一起使用，以获取返回引用类型的方法的返回值。 如果将 IL 偏移量传递给函数调用站点，此方法将返回一个或多个本机偏移量。 然后，调试器可以在函数中的这些本机偏移量上设置断点。 当调试器遇到其中一个断点时，可以将传递给此方法的同一 IL 偏移传递给 [ICorDebugILFrame3：： GetReturnValueForILOffset](icordebugilframe3-getreturnvalueforiloffset-method.md) 方法以获取返回值。 调试器随后应清除它所设置的所有断点。  
  
> [!WARNING]
> `ICorDebugCode3::GetReturnValueLiveOffset`和[ICorDebugILFrame3：： GetReturnValueForILOffset](icordebugilframe3-getreturnvalueforiloffset-method.md)方法只允许获取引用类型的返回值信息。 从值类型检索返回值信息 (也就是说，不支持从) 派生的所有类型 <xref:System.ValueType> 。  
  
 函数将返回 `HRESULT` 下表中显示的值。  
  
|`HRESULT` 值|说明|  
|---------------------|-----------------|  
|`S_OK`|成功。|  
|`CORDBG_E_INVALID_OPCODE`|给定的 IL 偏移量站点不是调用指令，或该函数返回 `void` 。|  
|`CORDBG_E_UNSUPPORTED`|给定的 IL 偏移量是正确的调用，但不支持返回类型来获取返回值。|  
  
 `ICorDebugCode3::GetReturnValueLiveOffset`方法仅适用于基于 x86 的和 AMD64 系统。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v451plus](../../../../includes/net-current-v451plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetReturnValueForILOffset 方法](icordebugilframe3-getreturnvalueforiloffset-method.md)
- [ICorDebugCode3 接口](icordebugcode3-interface.md)
