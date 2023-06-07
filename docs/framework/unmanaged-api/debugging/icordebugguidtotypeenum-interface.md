---
title: ICorDebugGuidToTypeEnum 接口
ms.date: 03/30/2017
api_name:
- ICorDebugGuidToTypeEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugGuidToTypeEnum
helpviewer_keywords:
- ICorDebugGuidToTypeEnum interface [.NET Framework debugging]
ms.assetid: aa32b12b-05fc-4ea8-a904-adae25034269
topic_type:
- apiref
ms.openlocfilehash: 149c5b09639c8809e736ade09566e7b1b530e3eb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705704"
---
# <a name="icordebugguidtotypeenum-interface"></a>ICorDebugGuidToTypeEnum 接口

提供一个枚举器，该枚举器定义一组 Guid 与它们对应的类型之间的映射（由 ICorDebugType 实例表示）。 此接口继承 ICorDebugEnum 接口中的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ICorDebugGuidToTypeEnum：： Next](icordebugguidtotypeenum-next-method.md)|获取指定数量的 [CorDebugGuidToTypeMapping](cordebugguidtotypemapping-structure.md) 实例，这些实例将 guid 映射到类型信息。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugGuidToTypeEnum`可以通过调用[ICorDebugAppDomain3：： GetCachedWinRTTypes](icordebugappdomain3-getcachedwinrttypes-method.md)方法来检索 interface 对象。 调试器可以调用此接口的 [下一](icordebugguidtotypeenum-next-method.md) 方法来检索 [CorDebugGuidToTypeMapping](cordebugguidtotypemapping-structure.md) 对象，这些对象表示在用于调用 [ICorDebugAppDomain3：： GetCachedWinRTTypes](icordebugappdomain3-getcachedwinrttypes-method.md) 方法的应用程序域中加载的 Windows 运行时类型的托管表示形式的映射。  
  
## <a name="requirements"></a>要求  

 **平台：** Windows 运行时  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
