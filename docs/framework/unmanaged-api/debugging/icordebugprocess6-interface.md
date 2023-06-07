---
title: “ICor调试进程6”接口
ms.date: 03/30/2017
ms.assetid: 34a10ac2-882c-4797-8369-f120e8e640c7
ms.openlocfilehash: ba70bab28eeddad6e3cf3c2b82b196a69ce68647
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732601"
---
# <a name="icordebugprocess6-interface"></a>“ICor调试进程6”接口

合理扩展 ICor调试进程界面，以启用解码托管调试事件（编码在本机异常调试事件和虚拟模块拆分中）等功能。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[DecodeEvent 方法](icordebugprocess6-decodeevent-method.md)|对封装于特殊构造的本机异常调试事件有效载荷中的托管调试事件进行解码。|  
|[EnableVirtualModuleSplitting 方法](icordebugprocess6-enablevirtualmodulesplitting-method.md)|启用或禁用虚拟模块拆分。|  
|[GetCode 方法](icordebugprocess6-getcode-method.md)|获取特定代码地址上的托管代码的相关信息。|  
|[GetExportStepInfo 方法](icordebugprocess6-getexportstepinfo-method.md)|提供运行时导出功能信息以帮助单步调试托管代码。|  
|[MarkDebuggerAttached 方法](icordebugprocess6-markdebuggerattached-method.md)|更改调试对象的内部状态，以便 .NET Framework 类库中的 <xref:System.Diagnostics.Debugger.IsAttached%2A?displayProperty=nameWithType> 方法返回 `true`。|  
|[“进程状态已更改”方法](icordebugprocess6-processstatechanged-method.md)|通知 [ICorDebug](icordebug-interface.md) 进程正在运行。|  
  
## <a name="remarks"></a>注解  
  
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
