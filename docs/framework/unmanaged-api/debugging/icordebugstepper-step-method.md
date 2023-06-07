---
title: ICorDebugStepper::Step 方法
ms.date: 03/30/2017
api_name:
- ICorDebugStepper.Step
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper::Step
helpviewer_keywords:
- Step method, ICorDebugStepper interface [.NET Framework debugging]
- ICorDebugStepper::Step method [.NET Framework debugging]
ms.assetid: 38c1940b-ada1-40ba-8295-4c0833744e1e
topic_type:
- apiref
ms.openlocfilehash: 234705e4495a1a582f3801ad1e645f923cd6f4b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727687"
---
# <a name="icordebugstepperstep-method"></a>ICorDebugStepper::Step 方法

使此 ICorDebugStepper 单步执行其包含线程，并根据需要继续单步执行线程中调用的函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Step (  
    [in] BOOL   bStepIn  
);  
```  
  
## <a name="parameters"></a>参数  

 `bStepIn`  
 中设置为 `true` 可单步执行在线程中调用的函数。 设置为 `false` 以逐过程执行函数。  
  
## <a name="remarks"></a>注解  

 当公共语言运行时执行此分档器帧中的下一个托管指令时，此步骤即告完成。 如果 `Step` 在不在托管代码中的分档器上调用，则在线程执行下一个托管代码指令时，步骤将完成。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
