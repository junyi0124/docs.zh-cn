---
title: ICorDebugCode4：： EnumerateVariableHomes 方法
ms.date: 03/30/2017
api_name:
- ICorDebugCode4.EnumerateVariableHomes
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode4::EnumerateVariableHomes
helpviewer_keywords:
- EnumerateVariableHomes method [.NET Framework debugging]
- ICorDebugCode4::EnumerateVariableHomes method [.NET Framework debugging]
ms.assetid: 802c01ff-8b80-4733-b6dd-03ab6ff7fa11
topic_type:
- apiref
ms.openlocfilehash: 6d58efa5629bb02158a275dec61c0313bca821a1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720745"
---
# <a name="icordebugcode4enumeratevariablehomes-method"></a>ICorDebugCode4：： EnumerateVariableHomes 方法

获取函数中局部变量和参数的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateVariableHomes(  
    [out] ICorDebugVariableHomeEnum **ppEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 指向 [ICorDebugVariableHomeEnum](icordebugvariablehomeenum-interface.md) 接口对象地址的指针，该对象是函数中局部变量和参数的枚举器。  
  
## <a name="remarks"></a>注解  

 [ICorDebugVariableHomeEnum](icordebugvariablehomeenum-interface.md)接口对象是从 "ICorDebugEnum" 接口派生的标准枚举器，该枚举器可用于枚举[ICorDebugVariableHome](icordebugvariablehome-interface.md)对象。 如果同一槽或参数索引的多个 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 对象在函数不同的点具有不同的主对象，则该集合可能包含多个这些对象。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugCode4 接口](icordebugcode4-interface.md)
- [调试接口](debugging-interfaces.md)
