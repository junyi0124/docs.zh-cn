---
title: ICorDebugRuntimeUnwindableFrame 接口
ms.date: 03/30/2017
api_name:
- ICorDebugUnwindableFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugUnwindableFrame
helpviewer_keywords:
- ICorDebugUnwindableFrame interface [.NET Framework debugging]
ms.assetid: cd6a3982-6ed3-4909-808d-a66055e813e0
topic_type:
- apiref
ms.openlocfilehash: 6619b8888588341f23a93b83865cd2e75cc9b3db
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712009"
---
# <a name="icordebugruntimeunwindableframe-interface"></a>ICorDebugRuntimeUnwindableFrame 接口

提供对非托管方法的支持，这些方法需要公共语言运行时 (CLR) 来展开帧。  
  
## <a name="remarks"></a>注解  

 `ICorDebugRuntimeUnwindableFrame` 是 ICorDebugFrame 接口的专用版本。 它由需要运行时展开当前堆栈上的帧的非托管方法使用。 现有的展开约定不起作用，因为它们不使用 JIT 编译代码。 当调试器发现不可展开帧时，它应使用 [ICorDebugStackWalk：： Next](icordebugstackwalk-next-method.md) 方法展开它，但它应执行检查。 当调试器收到时 `ICorDebugRuntimeUnwindableFrame` ，它可以调用 [ICorDebugStackWalk：： GetContext](icordebugstackwalk-getcontext-method.md) 方法来检索该帧的上下文。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
