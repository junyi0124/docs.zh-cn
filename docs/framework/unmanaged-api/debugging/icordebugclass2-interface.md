---
title: ICorDebugClass2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugClass2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass2
helpviewer_keywords:
- ICorDebugClass2 interface [.NET Framework debugging]
ms.assetid: 5416de70-43f2-4cdf-a11f-d570759c9c0c
topic_type:
- apiref
ms.openlocfilehash: ce3f289ae914817071fad5274c45d1e5fae71a06
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717976"
---
# <a name="icordebugclass2-interface"></a>ICorDebugClass2 接口

表示泛型类或具有 <xref:System.Type> 类型的方法参数的类。 此接口扩展 [ICorDebugClass](icordebugclass-interface.md)。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetParameterizedType 方法](icordebugclass2-getparameterizedtype-method.md)|获取此类的类型声明。|  
|[SetJMCStatus 方法](icordebugclass2-setjmcstatus-method.md)|对于此类的每个方法，设置一个值，该值指示该方法是否为用户定义的代码。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugClass 接口](icordebugclass-interface.md)
- [调试接口](debugging-interfaces.md)
