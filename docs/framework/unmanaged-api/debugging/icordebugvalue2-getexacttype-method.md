---
title: ICorDebugValue2::GetExactType 方法
ms.date: 03/30/2017
api_name:
- ICorDebugValue2.GetExactType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue2::GetExactType
helpviewer_keywords:
- ICorDebugValue2::GetExactType method [.NET Framework debugging]
- GetExactType method [.NET Framework debugging]
ms.assetid: 8e9aae1b-d1b7-4b6e-b577-6faf36dcec85
topic_type:
- apiref
ms.openlocfilehash: cb5bec66ab02de248109d8aaf444a93e67c2c6d2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720355"
---
# <a name="icordebugvalue2getexacttype-method"></a>ICorDebugValue2::GetExactType 方法

获取一个接口指针，该指针指向表示 <xref:System.Type> 此值的的 "ICorDebugType" 对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetExactType (  
    [out] ICorDebugType   **ppType  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppType`  
 弄指向对象地址的指针，该 `ICorDebugType` 对象表示 <xref:System.Type> 此 "ICorDebugValue2" 对象表示的值的。  
  
## <a name="remarks"></a>注解  

 一般识别 `GetExactType` 方法取代了 [ICorDebugObjectValue：： GetClass](icordebugobjectvalue-getclass-method.md) 和 [ICorDebugValue：： GetType](icordebugvalue-gettype-method.md) 方法，其中每个方法都返回值类型的相关信息。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
