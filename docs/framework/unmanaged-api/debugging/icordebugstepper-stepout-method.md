---
title: ICorDebugStepper::StepOut 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.StepOut
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::StepOut
helpviewer_keywords:
- ICorDebugStepper::StepOut method [.NET Framework debugging]
- StepOut method [.NET Framework debugging]
ms.assetid: aae0f48c-4ede-4256-9251-a7fc85a229dc
topic_type:
- apiref
ms.openlocfilehash: 1396e7a8ca61734a9363a9c852502290675249d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727661"
---
# <a name="icordebugstepperstepout-method"></a>ICorDebugStepper::StepOut 方法

使此 ICorDebugStepper 单步执行其包含线程，并在当前帧将控件返回到调用帧时完成。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StepOut ();  
```  
  
## <a name="remarks"></a>备注  

 `StepOut`操作将在从当前帧正常返回到调用帧后完成。  
  
 如果 `StepOut` 在非托管代码中调用，则当当前帧返回到调用它的托管代码时，步骤将完成。  
  
 在 .NET Framework 版本2.0 中，不要将 `StepOut` 与 STOP_UNMANAGED 标志集一起使用，因为它将失败。  (使用 [ICorDebugStepper：： SetUnmappedStopMask](icordebugstepper-setunmappedstopmask-method.md) 设置要单步执行的标志。 ) 互操作调试器必须自行跳出。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
