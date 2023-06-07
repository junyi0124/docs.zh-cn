---
title: ICorDebugILCode2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugILCode2
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: f9dc2afd-df8a-464d-bdbf-5af0a1d4bf85
topic_type:
- apiref
ms.openlocfilehash: b9289b5afc88c926ce585a4e620364cf2dc979d5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703325"
---
# <a name="icordebugilcode2-interface"></a>ICorDebugILCode2 接口

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 对 [ICorDebugILCode](icordebugilcode-interface.md) 接口进行逻辑扩展，以提供返回函数的局部变量签名的标记的方法，并将探查器检测到的中间语言 (il) 偏移量映射到原始方法的 il 偏移量。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetInstrumentedILMap 方法](icordebugilcode2-getinstrumentedilmap-method.md)|返回从探查器检测到的 IL 偏移量到此实例的原始方法的 IL 偏移量的映射。|  
|[GetLocalVarSigToken 方法](icordebugilcode2-getlocalvarsigtoken-method.md)|获取元数据标记，用于由此实例表示的函数的局部变量签名。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugILCode 接口](icordebugilcode-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
