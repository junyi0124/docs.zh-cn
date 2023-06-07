---
title: ICorDebugEval 接口
ms.date: 03/30/2017
api_name:
- ICorDebugEval
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval
helpviewer_keywords:
- ICorDebugEval interface [.NET Framework debugging]
ms.assetid: 3a5c9815-832d-47e1-b7f7-bbba135d7cf1
topic_type:
- apiref
ms.openlocfilehash: 5d8fd79b242f2b88b82c5c3d78dfe45d80f1194f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729780"
---
# <a name="icordebugeval-interface"></a>ICorDebugEval 接口

提供使调试器能够在正在调试的代码的上下文中执行代码的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Abort 方法](icordebugeval-abort-method.md)|中止此 `ICorDebugEval` 对象当前正在执行的计算。|  
|[CallFunction 方法](icordebugeval-callfunction-method.md)|设置对指定函数的调用。  (在 .NET Framework 版本2.0 中已过时;改 [为使用 ICorDebugEval2：： CallParameterizedFunction](icordebugeval2-callparameterizedfunction-method.md) ) |  
|[CreateValue 方法](icordebugeval-createvalue-method.md)|获取一个接口指针，该指针指向指定类型的 "ICorDebugValue" 对象，其初始值为零或 null。 .NET Framework 2.0 中 (过时;改 [为使用 ICorDebugEval2：： CreateValueForType](icordebugeval2-createvaluefortype-method.md) ) |  
|[GetResult 方法](icordebugeval-getresult-method.md)|获取一个接口指针，该指针指向 `ICorDebugValue` 包含计算结果的。|  
|[GetThread 方法](icordebugeval-getthread-method.md)|获取一个接口指针，该指针指向正在执行或将执行此计算的 "ICorDebugThread"。|  
|[IsActive 方法](icordebugeval-isactive-method.md)|获取一个值，该值指示此 `ICorDebugEval` 对象当前是否正在执行。|  
|[NewArray 方法](icordebugeval-newarray-method.md)|分配指定元素类型和维度的新数组。 .NET Framework 2.0 中 (过时;改 [为使用 ICorDebugEval2：： NewParameterizedArray](icordebugeval2-newparameterizedarray-method.md) ) |  
|[NewObject 方法](icordebugeval-newobject-method.md)|分配一个新的对象实例，并调用指定的构造函数方法。 .NET Framework 2.0 中 (过时;改 [为使用 ICorDebugEval2：： NewParameterizedObject](icordebugeval2-newparameterizedobject-method.md) ) |  
|[NewObjectNoConstructor 方法](icordebugeval-newobjectnoconstructor-method.md)|分配指定类型的新对象实例，而不尝试调用构造函数方法。 .NET Framework 2.0 中 (过时;改 [为使用 ICorDebugEval2：： NewParameterizedObjectNoConstructor](icordebugeval2-newparameterizedobjectnoconstructor-method.md) ) |  
|[NewString 方法](icordebugeval-newstring-method.md)|使用指定的内容分配新的字符串对象。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugEval`在用于执行计算的特定线程的上下文中创建对象。 给定计算中使用的所有对象和类型必须位于同一个应用程序域中。 该应用程序域不需要与该线程的当前应用程序域相同。 评估可以嵌套。  
  
 直到调试程序调用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md)并收到 [ICorDebugManagedCallback：： EvalComplete](icordebugmanagedcallback-evalcomplete-method.md) 回调后，计算的操作才会完成。 如果需要使用评估功能而不允许其他线程运行，请在调用[ICorDebugController：： Continue](icordebugcontroller-continue-method.md)之前，通过使用[ICorDebugController：： SetAllThreadsDebugState](icordebugcontroller-setallthreadsdebugstate-method.md)或[ICorDebugController：： Stop](icordebugcontroller-stop-method.md)挂起线程。  
  
 由于正在进行计算时用户代码正在运行，因此可能会发生任何调试事件，包括类加载和断点。 对于这些事件，调试器将接收回拨。 评估的状态将被视为正常程序状态的一部分。 堆栈链将是一个 `CHAIN_FUNC_EVAL` 链 (请参阅 "CorDebugStepReason" 枚举和 [ICorDebugChain：： GetReason](icordebugchain-getreason-method.md) 方法) 。 完整的调试器 API 将继续正常运行。  
  
 如果发生死锁或无限循环，用户代码可能永远无法完成。 在这种情况下，必须在恢复程序之前调用 [ICorDebugEval：： Abort](icordebugeval-abort-method.md) 。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
