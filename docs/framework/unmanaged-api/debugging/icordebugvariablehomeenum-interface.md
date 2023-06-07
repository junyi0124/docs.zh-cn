---
title: ICorDebugVariableHomeEnum 接口
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHomeEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHomeEnum
helpviewer_keywords:
- ICorDebugVariableHomeEnum interface [.NET Framework debugging]
ms.assetid: c312ae6d-c8dc-48d6-9f1e-ead515c88fdf
topic_type:
- apiref
ms.openlocfilehash: 8e8caad9f0fc60121dbd1c738a6024da3e4d02f6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725997"
---
# <a name="icordebugvariablehomeenum-interface"></a>ICorDebugVariableHomeEnum 接口

提供对函数中的局部变量和参数的枚举器。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Next 方法](icordebugvariablehomeenum-next-method.md)|获取指定数量的 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 实例，其中包含有关函数中的局部变量和参数的信息。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugVariableHomeEnum`接口实现 ICorDebugEnum 接口。  
  
 `ICorDebugVariableHomeEnum`实例通过调用[ICorDebugCode4：： EnumerateVariableHomes](icordebugcode4-enumeratevariablehomes-method.md)方法，使用[ICorDebugVariableHome](icordebugvariablehome-interface.md)实例进行填充。 集合中的每个 [ICorDebugVariableHome](icordebugvariablehome-interface.md) 实例都表示函数中的局部变量或自变量。 可以通过调用[ICorDebugVariableHomeEnum：： Next](icordebugvariablehomeenum-next-method.md)方法来枚举集合中的[ICorDebugVariableHome](icordebugvariablehome-interface.md)对象。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
- [调试接口](debugging-interfaces.md)
