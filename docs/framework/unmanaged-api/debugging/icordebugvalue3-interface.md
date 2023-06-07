---
title: ICorDebugValue3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugValue3
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugValue3
helpviewer_keywords:
- ICorDebugValue3 interface [.NET Framework debugging]
ms.assetid: 7d5385d3-f4a5-47c4-8478-a3513b5e9406
topic_type:
- apiref
ms.openlocfilehash: 419efc7f21f4ac2e68657b2a4d69a8690a2938d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722305"
---
# <a name="icordebugvalue3-interface"></a>ICorDebugValue3 接口

扩展了 "ICorDebugValue" 和 "ICorDebugValue2" 接口，为大于 2 GB 的数组提供支持。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetSize64 方法](icordebugvalue3-getsize64-method.md)|获取此对象的大小（以字节为单位） `ICorDebugValue3` 。|  
  
## <a name="remarks"></a>注解  

 [ICorDebugValue：： GetSize](icordebugvalue3-getsize64-method.md)方法返回的对象大小范围为0到2147483647字节。 在 .NET Framework 4.5 中，数组的大小可超过 2 GB。 `ICorDebugValue3`接口使您能够确定这些数组的大小。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
