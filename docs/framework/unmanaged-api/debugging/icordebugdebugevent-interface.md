---
title: “ICor调试调试事件”接口
ms.date: 03/30/2017
ms.assetid: a226737a-cb99-4e97-bd94-9a37094ded41
ms.openlocfilehash: d73857bd9d0d5dd9e5eff0c89dcc573ae0d93f0e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731873"
---
# <a name="icordebugdebugevent-interface"></a>“ICor调试调试事件”接口

定义所有 `ICorDebug` 调试事件派生的基接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetEventKind 方法](icordebugdebugevent-geteventkind-method.md)|指出该 `ICorDebugDebugEvent` 对象代表的事件类型。|  
|[GetThread 方法](icordebugdebugevent-getthread-method.md)|获取发生事件的线程。|  
  
## <a name="remarks"></a>注解  

 以下接口派生自 `ICorDebugDebugEvent` 接口：  
  
- [ICorDebugExceptionDebugEvent](icordebugexceptiondebugevent-interface.md)  
  
- [ICorDebugModuleDebugEvent](icordebugmoduledebugevent-interface.md)  
  
> [!NOTE]
> 此接口仅适用于 .NET Native。 尝试调用 `QueryInterface` 来检索接口指针会为 .NET Native 外部的 ICorDebug 方案返回 `E_NOINTERFACE`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
