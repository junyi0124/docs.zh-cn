---
title: ICorDebugStepper::SetUnmappedStopMask 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.SetUnmappedStopMask
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::SetUnmappedStopMask
helpviewer_keywords:
- ICorDebugStepper::SetUnmappedStopMask method [.NET Framework debugging]
- SetUnmappedStopMask method [.NET Framework debugging]
ms.assetid: b1211981-e90c-4e05-8def-fa18d85ad9ab
topic_type:
- apiref
ms.openlocfilehash: 50fad8b38a6b33d0ddbb2f0f20676296c3d66737
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95698723"
---
# <a name="icordebugsteppersetunmappedstopmask-method"></a>ICorDebugStepper::SetUnmappedStopMask 方法

设置一个值，该值指定执行过程中将暂停的未映射代码的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetUnmappedStopMask (  
    [in] CorDebugUnmappedStop   mask  
);  
```  
  
## <a name="parameters"></a>参数  

 `mask`  
 中CorDebugUnmappedStop 枚举的一个值，该值指定调试器将在其中暂停执行的未映射代码的类型。  
  
 默认值为 STOP_OTHER_UNMAPPED。 值 STOP_UNMANAGED 仅适用于互操作调试。  
  
## <a name="remarks"></a>注解  

 当调试器找到与 Microsoft 中间语言 (MSIL) 无对应映射的实时 (JIT) 编译时，如果指定了该类型的未映射代码的标志，则将暂停执行;否则，单步执行会透明地继续。  
  
 如果调试器不使用分档器来输入方法，则不一定要逐过程执行非映射代码。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
