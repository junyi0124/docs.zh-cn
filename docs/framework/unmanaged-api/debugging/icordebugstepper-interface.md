---
title: ICorDebugStepper 接口
ms.date: 03/30/2017
api_name:
- ICorDebugStepper
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugStepper
helpviewer_keywords:
- ICorDebugStepper interface [.NET Framework debugging]
ms.assetid: ed8364eb-f01b-46f6-b5e3-5dda9cae2dfe
topic_type:
- apiref
ms.openlocfilehash: 8b5bbb65034e5b715532397c9ecc650da9aee912
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718288"
---
# <a name="icordebugstepper-interface"></a>ICorDebugStepper 接口

表示在代码执行过程中由调试器执行的一个步骤。此步骤作为命令颁发和完成之间的标识符使用，可以实现取消对某个步骤的执行。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Deactivate 方法](icordebugstepper-deactivate-method.md)|使此 `ICorDebugStepper` 方法取消它收到的最后一个步骤命令。|  
|[IsActive 方法](icordebugstepper-isactive-method.md)|获取一个值，该值指示此是否 `ICorDebugStepper` 当前正在执行步骤。|  
|[SetInterceptMask 方法](icordebugstepper-setinterceptmask-method.md)|设置一个 CorDebugIntercept 值，该值指定要单步执行的代码的类型。|  
|[SetRangeIL 方法](icordebugstepper-setrangeil-method.md)|设置一个值，该值指示对 [ICorDebugStepper：： StepRange](icordebugstepper-steprange-method.md) 的调用是传递与本机代码相关的参数值，还是传递给 Microsoft 中间语言 (MSIL) 正在逐步执行的方法的代码。|  
|[SetUnmappedStopMask 方法](icordebugstepper-setunmappedstopmask-method.md)|设置一个 CorDebugUnmappedStop 值，该值指定将在其中停止执行的未映射代码的类型。|  
|[Step 方法](icordebugstepper-step-method.md)|使此 `ICorDebugStepper` 单步执行其包含线程，并根据需要继续单步执行线程中调用的函数。|  
|[StepOut 方法](icordebugstepper-stepout-method.md)|使此在 `ICorDebugStepper` 其包含线程中单步执行，并在当前帧将控件返回到调用帧时完成。|  
|[StepRange 方法](icordebugstepper-steprange-method.md)|使此操作在 `ICorDebugStepper` 其包含线程中单步执行，并在到达指定范围最后一个以上的代码时返回。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugStepper`接口具有以下用途：  
  
- 它充当发出的步骤命令与完成该命令之间的标识符。  
  
- 它提供一个中心接口，用于封装可执行的所有单步执行。  
  
- 它提供了一种提前取消单步执行操作的方法。  
  
 每个线程可以有多个分档器。 例如，在单步执行某个函数时可能会命中断点，用户可能希望在该函数中启动新的单步执行操作。 调试器负责确定如何处理这种情况。 调试器可能需要取消原始单步执行操作或嵌套两个操作。 `ICorDebugStepper`接口支持这两种选择。  
  
 如果公共语言运行时 (CLR) 进行跨线程的已封送调用，则分档器可以在线程之间迁移。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
