---
title: ICorDebugModuleDebugEvent 接口
ms.date: 03/30/2017
ms.assetid: 41950c52-1ac8-4212-b814-c77e20879f91
ms.openlocfilehash: 62d419a193cff000e1dd748d0cbb6b61775a81aa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695811"
---
# <a name="icordebugmoduledebugevent-interface"></a>ICorDebugModuleDebugEvent 接口

扩展 [ICorDebugDebugEvent](icordebugdebugevent-interface.md) 接口以支持模块级事件。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetModule 方法](icordebugmoduledebugevent-getmodule-method.md)|获取刚加载或卸载的合并模块。|  
  
## <a name="remarks"></a>注解  

 [MODULE_LOADED](cordebugdebugeventkind-enumeration.md)和[MODULE_UNLOADED](cordebugdebugeventkind-enumeration.md)事件类型实现此接口。  
  
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
