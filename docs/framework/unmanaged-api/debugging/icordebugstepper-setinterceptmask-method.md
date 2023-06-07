---
title: ICorDebugStepper::SetInterceptMask 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.SetInterceptMask
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::SetInterceptMask
helpviewer_keywords:
- SetInterceptMask method [.NET Framework debugging]
- ICorDebugStepper::SetInterceptMask method [.NET Framework debugging]
ms.assetid: 6245e2ae-5cc2-43ff-8cc1-71953d12113a
topic_type:
- apiref
ms.openlocfilehash: 814bf87039ef57056f13994af1b873f8f57c7804
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718210"
---
# <a name="icordebugsteppersetinterceptmask-method"></a>ICorDebugStepper::SetInterceptMask 方法

设置一个值，该值指定要单步执行的代码的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetInterceptMask (  
    [in] CorDebugIntercept    mask  
);  
```  
  
## <a name="parameters"></a>参数  

 `mask`  
 中CorDebugIntercept 枚举的值的组合，用于指定代码的类型。  
  
## <a name="remarks"></a>注解  

 如果设置了侦听器的位，则遇到给定类型的截取代码时，分档器将完成。 如果清除该位，则将跳过拦截代码。  
  
 此 `SetInterceptMask` 方法可能与 [ICorDebugStepper：： SetUnmappedStopMask](icordebugstepper-setunmappedstopmask-method.md) (从用户的观点) 交互。 例如，如果唯一可见的 (是：类初始化代码的非内部) 部分缺少映射信息，并且未设置 STOP_NO_MAPPING_INFO (参见 [ICorDebugStepper：： SetUnmappedStopMask](icordebugstepper-setunmappedstopmask-method.md) 方法和 CorDebugUnmappedStop 枚举) ，则分档器将逐过程执行类初始化。 默认情况下，将只使用枚举的 INTERCEPT_NONE 值 `CorDebugIntercept` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
