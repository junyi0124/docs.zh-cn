---
title: ICorDebugValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue
helpviewer_keywords:
- ICorDebugValue interface [.NET Framework debugging]
ms.assetid: b2f7007f-c446-4b18-aed1-a25cff8aee31
topic_type:
- apiref
ms.openlocfilehash: 7d3c35ed6cda637e3b885afe089ddfa590d51076
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683597"
---
# <a name="icordebugvalue-interface"></a>ICorDebugValue 接口

表示正在调试的进程中的值。 该值可以是读取或写入值。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateBreakpoint 方法](icordebugvalue-createbreakpoint-method.md)|目前未实现此方法。|  
|[GetAddress 方法](icordebugvalue-getaddress-method.md)|获取此对象的地址，该地址正在进行 `ICorDebugValue` 调试。|  
|[GetSize 方法](icordebugvalue-getsize-method.md)|获取此对象的大小（以字节为单位） `ICorDebugValue` 。|  
|[GetType 方法](icordebugvalue-gettype-method.md)|获取此对象的基元类型 `ICorDebugValue` 。|  
  
## <a name="remarks"></a>注解  

 通常，值对象的所有权在返回时传递。 当对象使用完对象后，接收方负责从对象中删除引用。  
  
 根据从何处检索值，在恢复进程后，值可能不会保持有效。 因此，通常情况下，不应在对 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md) 方法的调用中保存值。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugValue3 接口](icordebugvalue3-interface.md)
- [调试接口](debugging-interfaces.md)
