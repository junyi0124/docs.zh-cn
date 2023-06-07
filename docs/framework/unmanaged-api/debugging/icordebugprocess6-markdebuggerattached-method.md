---
title: ICorDebugProcess6::MarkDebuggerAttached 方法
ms.date: 03/30/2017
ms.assetid: bf94f090-5265-4112-8e57-5b4e20e070d0
ms.openlocfilehash: c6543a89a375d4a2887dbe8cff56d66a15650811
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732588"
---
# <a name="icordebugprocess6markdebuggerattached-method"></a>ICorDebugProcess6::MarkDebuggerAttached 方法

更改调试对象的内部状态，以便 .NET Framework 类库中的 <xref:System.Diagnostics.Debugger.IsAttached%2A?displayProperty=nameWithType> 方法返回 `true`。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT MarkDebuggerAttached(  
    BOOL fIsAttached  
);  
```  
  
## <a name="parameters"></a>参数  

 `fIsAttached`  
 如果 `true` 方法指出已连接调试程序，则为<xref:System.Diagnostics.Debugger.IsAttached%2A?displayProperty=nameWithType>；否则即为 `false`。  
  
## <a name="return-value"></a>返回值  

 该方法可以返回下表所列的值。  
  
|返回值|说明|  
|------------------|-----------------|  
|`S_OK`|调试对象已成功更新。|  
|`CORDBG_E_MODULE_NOT_LOADED`|程序集包含未加载的 <xref:System.Diagnostics.Debugger.IsAttached%2A?displayProperty=nameWithType> 方法或一些其他错误（例如元数据丢失），无法被识别。<br /><br /> 该错误很常见，没有危害。 你应当在其他程序集加载时再次调用方法。|  
|其他失败 `HRESULT` 值。|其他值可能表示错误调试程序或编译器组件。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试进程6”接口](icordebugprocess6-interface.md)
- [调试接口](debugging-interfaces.md)
