---
title: ICorDebugNativeFrame2::IsMatchingParentFrame 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame2.IsMatchingParentFrame Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame2::IsMatchingParentFrame
helpviewer_keywords:
- IsMatchingParentFrame method [.NET Framework debugging]
- ICorDebugNativeFrame2::IsMatchingParentFrame method [.NET Framework debugging]
ms.assetid: d2ca20db-df22-4528-a0dd-a09ea62c8998
topic_type:
- apiref
ms.openlocfilehash: 213bee96531fa0bbc9bf0ae76b2505019833abfc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724697"
---
# <a name="icordebugnativeframe2ismatchingparentframe-method"></a>ICorDebugNativeFrame2::IsMatchingParentFrame 方法

确定指定的帧是否为当前帧的父级。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsMatchingParentFrame([in] ICorDebugNativeFrame2  
                                      *pPotentialParentFrame,  
                              [out] BOOL *pIsParent);  
```  
  
## <a name="parameters"></a>参数  

 `pPotentialParentFrame`  
 中一个指针，指向要为父状态计算的帧对象。  
  
 `pIsParent`  
 [out] `true` 如果 `pPotentialParentFrame` 为当前帧的父级，则为; 否则为 `false` 。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|父状态已成功返回。|  
|E_FAIL|无法返回父状态。|  
|E_INVALIDARG|`pPotentialParentFrame` 或 `pIsParent` 为 null。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 `IsMatchingParentFrame``true`如果传递给方法的帧对象是在其上调用方法的 frame 对象的父对象，则返回。 如果在不是指定帧的子级的帧上调用方法，则会返回错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugNativeFrame2 接口](icordebugnativeframe2-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
