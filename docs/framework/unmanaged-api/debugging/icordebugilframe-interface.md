---
title: ICorDebugILFrame 接口
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame
helpviewer_keywords:
- ICorDebugILFrame interface [.NET Framework debugging]
ms.assetid: d5cf5056-da4d-4629-914d-afe42a5393df
topic_type:
- apiref
ms.openlocfilehash: 4f34fdf9a0eeb47e027cc874afee5bd04f5bd9bc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95712386"
---
# <a name="icordebugilframe-interface"></a>ICorDebugILFrame 接口

表示 Microsoft 中间语言 (MSIL) 代码的堆栈帧。 此接口是 ICorDebugFrame 接口的子类。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CanSetIP 方法](icordebugilframe-cansetip-method.md)|获取一个值，该值指示是否可以安全地将指令指针设置为指定的偏移位置。|  
|[EnumerateArguments 方法](icordebugilframe-enumeratearguments-method.md)|获取此帧中的参数的枚举数。|  
|[EnumerateLocalVariables 方法](icordebugilframe-enumeratelocalvariables-method.md)|获取此帧中局部变量的枚举数。|  
|[GetArgument 方法](icordebugilframe-getargument-method.md)|获取此 MSIL 堆栈帧中的指定参数的值。|  
|[GetIP 方法](icordebugilframe-getip-method.md)|获取指令指针的值和按位组合值，它描述如何获取指令指针的值。|  
|[GetLocalVariable 方法](icordebugilframe-getlocalvariable-method.md)|获取此 MSIL 堆栈帧中的指定局部变量的值。|  
|[GetStackDepth 方法](icordebugilframe-getstackdepth-method.md)|未实现。|  
|[GetStackValue 方法](icordebugilframe-getstackvalue-method.md)|未实现。|  
|[SetIP 方法](icordebugilframe-setip-method.md)|将指令指针设置为指向 MSIL 代码中的指定偏移位置。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugILFrame`接口是专用的 ICorDebugFrame 接口。 它可用于 MSIL 代码帧或实时 (JIT) 编译的帧。 JIT 编译的帧实现 `ICorDebugILFrame` 接口和 ICorDebugNativeFrame 接口。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
