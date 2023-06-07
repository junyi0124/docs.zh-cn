---
title: ICorDebugNativeFrame2::IsChild 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame2.IsChild Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame2::IsChild
helpviewer_keywords:
- IsChild method [.NET Framework debugging]
- ICorDebugNativeFrame2::IsChild method [.NET Framework debugging]
ms.assetid: 9e2aae09-49cb-4fbd-81e5-e29cd864a88b
topic_type:
- apiref
ms.openlocfilehash: 0d65849aba08c7d143a6977e7dfb8cff85274a64
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695564"
---
# <a name="icordebugnativeframe2ischild-method"></a>ICorDebugNativeFrame2::IsChild 方法

确定当前帧是否为子框架。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsChild([out] BOOL * pIsChild);  
```  
  
## <a name="parameters"></a>参数  

 `pIsChild`  
 弄指定当前帧是否为子框架的布尔值。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|已成功返回子状态。|  
|E_FAIL|无法返回子状态。|  
|E_INVALIDARG|`pIsChild` 为 null。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 `IsChild` `true` 如果调用方法的帧对象是另一帧的子对象，则该方法返回。 如果是这种情况，请使用 [IsMatchingParentFrame](icordebugnativeframe2-ismatchingparentframe-method.md) 方法检查帧是否为其父级。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugNativeFrame2 接口](icordebugnativeframe2-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
