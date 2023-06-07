---
title: ICorDebugVariableSymbol::SetValue 方法
ms.date: 03/30/2017
ms.assetid: 4609418d-71fa-44bc-9618-4d529d25cabb
ms.openlocfilehash: 3ef9b9b9a095d6d6026ffc38873661355d660488
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707836"
---
# <a name="icordebugvariablesymbolsetvalue-method"></a>ICorDebugVariableSymbol::SetValue 方法

将字节数组的值分配给变量。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetValue(  
   [in] ULONG32 offset,  
   [in] DWORD threadID,  
   [in] ULONG32 cbContext,  
   [in, size_is(cbContext)] BYTE context[],  
   [in] ULONG32 cbValue,  
   [in, size_is(cbValue)] BYTE pValue[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `offset`  
 [in] 设置值时变量中的起始偏移量。 在对象中写入成员字段时，则使用此参数。  
  
 `threadID`  
 [in] 其上下文必须更新以反映新值的线程的线程标识符。  
  
 `cbContext`  
 [in] 线程上下文的大小（以字节为单位）。  
  
 `context`  
 [in] 用于写入值的线程上下文。  
  
 `cbValue`  
 [in] `pValue` 缓冲区的大小（以字节为单位）。  
  
 `pValue`  
 [in] 包含要设置的值的缓冲区。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableSymbol 接口](icordebugvariablesymbol-interface.md)
- [调试接口](debugging-interfaces.md)
