---
title: ICorDebugModule 接口
ms.date: 03/30/2017
api_name:
- ICorDebugModule
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule
helpviewer_keywords:
- ICorDebugModule interface [.NET Framework debugging]
ms.assetid: 32e4d6fa-e5a3-413e-9166-d5e2871d3114
topic_type:
- apiref
ms.openlocfilehash: 86e17b48bc491c45f8b46be23ab626dc1f2a6962
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709838"
---
# <a name="icordebugmodule-interface"></a>ICorDebugModule 接口

表示公共语言运行时 (CLR) 模块，该模块可以是可执行文件或动态链接库 (DLL) 。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateBreakpoint 方法](icordebugmodule-createbreakpoint-method.md)|未实现。|  
|[EnableClassLoadCallbacks 方法](icordebugmodule-enableclassloadcallbacks-method.md)|确定是否为此模块调用 [ICorDebugManagedCallback：： LoadClass](icordebugmanagedcallback-loadclass-method.md) 和 [ICorDebugManagedCallback：： UnloadClass](icordebugmanagedcallback-unloadclass-method.md) 回调。|  
|[EnableJITDebugging 方法](icordebugmodule-enablejitdebugging-method.md)|确定实时 (JIT) 编译器是否保留此模块内方法的调试信息。|  
|[GetAssembly 方法](icordebugmodule-getassembly-method.md)|获取包含此模块的程序集。|  
|[GetBaseAddress 方法](icordebugmodule-getbaseaddress-method.md)|获取模块的基址。|  
|[GetClassFromToken 方法](icordebugmodule-getclassfromtoken-method.md)|从元数据中获取 ICorDebugClass。|  
|[GetEditAndContinueSnapshot 方法](icordebugmodule-geteditandcontinuesnapshot-method.md)|已弃用。|  
|[GetFunctionFromRVA 方法](icordebugmodule-getfunctionfromrva-method.md)|未实现。|  
|[GetFunctionFromToken 方法](icordebugmodule-getfunctionfromtoken-method.md)|获取元数据标记指定的函数。|  
|[GetGlobalVariableValue 方法](icordebugmodule-getglobalvariablevalue-method.md)|获取指定全局变量的值对象。|  
|[GetMetaDataInterface 方法](icordebugmodule-getmetadatainterface-method.md)|获取可用于检查模块的元数据的元数据接口指针。|  
|[GetName 方法](icordebugmodule-getname-method.md)|获取模块的文件名。|  
|[GetProcess 方法](icordebugmodule-getprocess-method.md)|获取此模块的包含进程。|  
|[GetSize 方法](icordebugmodule-getsize-method.md)|获取模块的大小（以字节为单位）。|  
|[GetToken 方法](icordebugmodule-gettoken-method.md)|获取此模块的表项的标记。|  
|[IsDynamic 方法](icordebugmodule-isdynamic-method.md)|指示模块是否为动态模块。|  
|[IsInMemory 方法](icordebugmodule-isinmemory-method.md)|指示此模块是否仅存在于内存中。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebug 接口](icordebug-interface.md)
- [调试接口](debugging-interfaces.md)
