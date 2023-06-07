---
title: ICorDebugComObjectValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugComObjectValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugComObjectValue
helpviewer_keywords:
- ICorDebugComObjectValue interface [.NET Framework debugging]
ms.assetid: 505a7f6c-d92b-42b4-b539-433f5102ea9b
topic_type:
- apiref
ms.openlocfilehash: 40df1416e68c86efe6d404119cb37277fe21ac56
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677539"
---
# <a name="icordebugcomobjectvalue-interface"></a>ICorDebugComObjectValue 接口

提供方法以检索与运行时可调用包装关联的信息 (RCW) 。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetCachedInterfacePointers 方法](icordebugcomobjectvalue-getcachedinterfacepointers-method.md)|获取缓存在当前 RCW 上的原始接口指针。|  
|[GetCachedInterfaceTypes 方法](icordebugcomobjectvalue-getcachedinterfacetypes-method.md)|为当前对象的大小写或用作的接口类型提供枚举器。|  
  
## <a name="remarks"></a>注解  

 若要检查 "ICorDebugValue" 接口的实例是否表示 RCW，调试程序 `QueryInterface` 使用 "ICorDebugValue" 调用 `IID_ICorDebugComObjectValue` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
