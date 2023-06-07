---
title: ICorDebugNativeFrame 接口
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame
helpviewer_keywords:
- ICorDebugNativeFrame interface [.NET Framework debugging]
ms.assetid: 04819c58-7246-4b32-befb-680cf1dbc436
topic_type:
- apiref
ms.openlocfilehash: 043dc0fdd5218d7bc6b80428d1eb891b3f01ee8c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695512"
---
# <a name="icordebugnativeframe-interface"></a>ICorDebugNativeFrame 接口

用于本机帧的 ICorDebugFrame 的专用实现。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CanSetIP 方法](icordebugnativeframe-cansetip-method.md)|获取一个值，该值指示是否可以安全地将指令指针设置为本机代码中的指定偏移位置。|  
|[GetIP 方法](icordebugnativeframe-getip-method.md)|获取堆栈帧到本机代码的偏移量。|  
|[GetLocalDoubleRegisterValue 方法](icordebugnativeframe-getlocaldoubleregistervalue-method.md)|获取一个指向 ICorDebugValue 的指针，该指针表示存储在本机帧的两个内存寄存器中的参数或局部变量的值。|  
|[GetLocalMemoryRegisterValue 方法](icordebugnativeframe-getlocalmemoryregistervalue-method.md)|获取一个指向的指针， `ICorDebugValue` 该指针表示本地变量的值，其中低位存储在指定的寄存器中，高位存储于指定的内存地址。|  
|[GetLocalMemoryValue 方法](icordebugnativeframe-getlocalmemoryvalue-method.md)|获取一个指向的指针 `ICorDebugValue` ，该指针表示存储在指定内存地址中的局部变量的值。|  
|[GetLocalRegisterMemoryValue 方法](icordebugnativeframe-getlocalregistermemoryvalue-method.md)|获取一个指向的指针， `ICorDebugValue` 该指针表示本地变量的值，其中高位存储在指定的寄存器中，低位存储在指定的内存地址|  
|[GetLocalRegisterValue 方法](icordebugnativeframe-getlocalregistervalue-method.md)|获取一个指向的指针 `ICorDebugValue` ，该指针表示存储在指定本机寄存器中的参数或局部变量的值。|  
|[GetRegisterSet 方法](icordebugnativeframe-getregisterset-method.md)|获取一个指针，该指针指向表示此的注册集的 [ICorDebugRegisterSet](icordebugregisterset-interface.md) `ICorDebugNativeFrame` 。|  
|[SetIP 方法](icordebugnativeframe-setip-method.md)|将指令指针设置为本机代码中的指定偏移位置。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
