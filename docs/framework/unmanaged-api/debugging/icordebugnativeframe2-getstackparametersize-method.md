---
title: ICorDebugNativeFrame2::GetStackParameterSize 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame2.GetStackParameterSize Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame2::GetStackParameterSize
helpviewer_keywords:
- ICorDebugNativeFrame2::GetStackParameterSize method [.NET Framework debugging]
- GetStackParameterSize method [.NET Framework debugging]
ms.assetid: f6a449c8-a941-43ba-9a90-c98b29ae3c36
topic_type:
- apiref
ms.openlocfilehash: 21af3980de9b5a768b6af9a8aca74b693c7ac528
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695486"
---
# <a name="icordebugnativeframe2getstackparametersize-method"></a>ICorDebugNativeFrame2::GetStackParameterSize 方法

返回 x86 操作系统上堆栈上的参数的累积大小。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetStackParameterSize([out] ULONG32 * pSize)  
```  
  
## <a name="parameters"></a>参数  

 `pSize`  
 弄指向堆栈上参数的累计大小的指针。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|堆栈大小已成功返回。|  
|S_FALSE|`GetStackParameterSize` 在非 x86 平台上调用了。|  
|E_FAIL|`The size of the parameters could not be returned`.|  
|E_INVALIDARG|`pSize` 为 `null` 。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 [ICorDebugStackWalk](icordebugstackwalk-interface.md)方法不调整推送到堆栈上的参数的堆栈指针。 相反，你可以使用返回的值 `GetStackParameterSize` 调整堆栈指针，使其成为本机展开器的种子，这会调整参数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugNativeFrame2 接口](icordebugnativeframe2-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
