---
title: ICorDebugVariableHomeEnum：： Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHomeEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHomeEnum::Next
helpviewer_keywords:
- ICorDebugVariableHomeEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugVariableHomeEnum interface [.NET Framework debugging]
ms.assetid: eb9ea96c-5b58-4655-8104-094fc8b393b8
topic_type:
- apiref
ms.openlocfilehash: 4ef4ed19033b0857b9970ee8103bbd92f383898c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679528"
---
# <a name="icordebugvariablehomeenumnext-method"></a>ICorDebugVariableHomeEnum：： Next 方法

获取指定数量的 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 实例，其中包含有关函数中的局部变量和参数的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next(  
    [in] ULONG celt,  
    [out, size_is(celt), length_is(*pceltFetched)] ICorDebugVariableHome *homes[],  
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 [in] 要检索的对象数。  
  
 `homes`  
 指针的数组，其中每个都指向一个 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 对象，该对象提供有关函数的局部变量或自变量的信息。  
  
 `pceltFetched`  
 弄对象中实际返回的实例数。  
  
## <a name="return-value"></a>返回值  

 方法返回以下值。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|`S_OK`|该方法已成功完成。|  
|`S_FALSE`|检索到的实际实例数小于所请求的 `pceltFetched` 实例数。|  
  
## <a name="remarks"></a>注解  

 [ICorDebugVariableHomeEnum：： Next](icordebugvariablehomeenum-next-method.md)方法 `celt` 从枚举器的当前位置开始检索最多对象。 当方法返回时， `pceltFetched` 包含检索到的对象的实际数目。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableHomeEnum 接口](icordebugvariablehomeenum-interface.md)
- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
