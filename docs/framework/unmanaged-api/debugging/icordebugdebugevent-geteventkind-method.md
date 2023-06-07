---
title: ICorDebugDebugEvent::GetEventKind 方法
ms.date: 03/30/2017
ms.assetid: c37aaceb-c948-46bd-a943-08be4cbb76f4
ms.openlocfilehash: 7876dc6ec5ecee52b64e54e1c42533f8348e4dc4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713673"
---
# <a name="icordebugdebugeventgeteventkind-method"></a>ICorDebugDebugEvent::GetEventKind 方法

指出该 `ICorDebugDebugEvent` 对象代表的事件类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetEventKind(  
    [out]CorDebugDebugEventKind *pDebugEventKind  
);  
```  
  
## <a name="parameters"></a>参数  

 p调试事件类型  
 指向指示事件类型的 [CorDebugDebugEventKind](cordebugdebugeventkind-enumeration.md) 枚举成员的指针。  
  
## <a name="remarks"></a>注解  

 基于 `pDebugEventKind` 值，可调用 `QueryInterface` 以获取包含其他数据的精确度更高的调试事件接口。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试调试事件”接口](icordebugdebugevent-interface.md)
- [调试接口](debugging-interfaces.md)
