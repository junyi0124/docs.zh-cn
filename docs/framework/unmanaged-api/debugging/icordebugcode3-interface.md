---
title: ICorDebugCode3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugCode3
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode3
helpviewer_keywords:
- ICorDebugCode3 interface [.NET Framework debugging]
ms.assetid: 70f07c9e-0614-4bee-ac34-09fe6c51c5a9
topic_type:
- apiref
ms.openlocfilehash: 609cd557db71fac53aadf613534a23e988b14bde
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720750"
---
# <a name="icordebugcode3-interface"></a>ICorDebugCode3 接口

提供一个方法，该方法将 "ICorDebugCode" 和 "ICorDebugCode2" 扩展为提供有关托管返回值的信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetReturnValueLiveOffset 方法](icordebugcode3-getreturnvalueliveoffset-method.md)|对于指定的 IL 偏移量，获取应放置断点的本机偏移量，以便调试器可以从函数中获取返回值。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v451plus](../../../../includes/net-current-v451plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugILFrame3 接口](icordebugilframe3-interface.md)
- [调试接口](debugging-interfaces.md)
