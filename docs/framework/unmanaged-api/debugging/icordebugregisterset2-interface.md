---
title: ICorDebugRegisterSet2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugRegisterSet2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugRegisterSet2
helpviewer_keywords:
- ICorDebugRegisterSet2 interface [.NET Framework debugging]
ms.assetid: d7fbccbf-3b6b-4db8-a96d-768e1cb6b1a6
topic_type:
- apiref
ms.openlocfilehash: c04bb3a7584fcb783af929e87713dbec67ad705f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712311"
---
# <a name="icordebugregisterset2-interface"></a>ICorDebugRegisterSet2 接口

为包含64个以上注册的硬件平台扩展了 [ICorDebugRegisterSet](icordebugregisterset-interface.md) 接口的功能。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetRegisters 方法](icordebugregisterset2-getregisters-method.md)|获取计算机上每个寄存器 (的值，该计算机当前正在执行位掩码指定的代码) 。|  
|[GetRegistersAvailable 方法](icordebugregisterset2-getregistersavailable-method.md)|获取提供可用寄存器的位图的字节数组。|  
|[SetRegisters 方法](icordebugregisterset2-setregisters-method.md)|在 .NET Framework 版本2.0 中未实现。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)
