---
title: ICLRDebugging::OpenVirtualProcess 方法
ms.date: 03/30/2017
api_name:
- ICLRDebugging.OpenVirtualProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDebugging::OpenVirtualProcess
helpviewer_keywords:
- OpenVirtualProcess method [.NET Framework debugging]
- ICLRDebugging::OpenVirtualProcess method [.NET Framework debugging]
ms.assetid: e8ab7c41-d508-4ed9-8a31-ead072b5a314
topic_type:
- apiref
ms.openlocfilehash: 2edd7f628e17c8dc6cbcbb577d06269ba8c64cb1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723530"
---
# <a name="iclrdebuggingopenvirtualprocess-method"></a>ICLRDebugging::OpenVirtualProcess 方法

获取 ICorDebugProcess 接口，该接口对应于进程中加载的公共语言运行时 (CLR) 模块。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OpenVirtualProcess(  
    [in] ULONG64 moduleBaseAddress,  
    [in] IUnknown * pDataTarget,  
    [in] ICLRDebuggingLibraryProvider * pLibraryProvider,  
    [in] CLR_DEBUGGING_VERSION * pMaxDebuggerSupportedVersion,  
    [in] REFIID riidProcess,  
    [out, iid_is(riidProcess)] IUnknown ** ppProcess,  
    [in, out] CLR_DEBUGGING_VERSION * pVersion,  
    [out] CLR_DEBUGGING_PROCESS_FLAGS * pdwFlags);  
```  
  
## <a name="parameters"></a>参数  

 `moduleBaseAddress`  
 中目标进程中的模块的基址。 如果指定的模块不是 CLR 模块，则将返回 COR_E_NOT_CLR。  
  
 `pDataTarget`  
 中一种数据目标抽象，允许托管调试器检查进程状态。 调试器必须实现 [ICorDebugDataTarget](icordebugdatatarget-interface.md) 接口。 应该实现 [ICLRDebuggingLibraryProvider](iclrdebugginglibraryprovider-interface.md) 接口以支持在计算机上未本地安装正在调试的 CLR 的情况。  
  
 `pLibraryProvider`  
 中一种库提供程序回调接口，允许根据需要定位和加载特定于版本的调试库。 仅当或不为时，才需要此参数 `ppProcess` `pFlags` `null` 。  
  
 `pMaxDebuggerSupportedVersion`  
 中此调试器可以调试的 CLR 的最高版本。 你应从该调试器支持的最新 CLR 版本中指定主版本、次版本和内部版本，并将修订号设置为65535，以适应未来的就地 CLR 服务版本。  
  
 `riidProcess`  
 中要检索的 ICorDebugProcess 接口的 ID。 目前，唯一接受的值是 IID_CORDEBUGPROCESS3、IID_CORDEBUGPROCESS2 和 IID_CORDEBUGPROCESS。  
  
 `ppProcess`  
 弄指向由标识的 COM 接口的指针 `riidProcess` 。  
  
 `pVersion`  
 [in，out]CLR 的版本。 对于输入，此值可以为 `null` 。 它还可以指向 [CLR_DEBUGGING_VERSION](clr-debugging-version-structure.md) 结构，在这种情况下， `wStructVersion` 必须将结构的字段初始化为0，)  (零。  
  
 输出时， `CLR_DEBUGGING_VERSION` 将用 CLR 的版本信息填充返回的结构。  
  
 `pdwFlags`  
 弄有关指定运行时的信息性标志。 有关标志的说明，请参阅 [CLR_DEBUGGING_PROCESS_FLAGS](clr-debugging-process-flags-enumeration.md) 主题。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`pDataTarget` 为 `null`。|  
|CORDBG_E_LIBRARY_PROVIDER_ERROR|[ICLRDebuggingLibraryProvider](iclrdebugginglibraryprovider-interface.md)回调返回错误或未提供有效的句柄。|  
|CORDBG_E_MISSING_DATA_TARGET_INTERFACE|`pDataTarget` 对于此版本的运行时，不实现所需的数据目标接口。|  
|CORDBG_E_NOT_CLR|指示的模块不是 CLR 模块。 当由于内存已损坏、该模块不可用或 CLR 版本高于填充码版本而无法检测到 CLR 模块时，也会返回此 HRESULT。|  
|CORDBG_E_UNSUPPORTED_DEBUGGING_MODEL|此运行时版本不支持此调试模型。 目前，CLR 版本在 .NET Framework 4 之前不支持调试模型。 `pwszVersion`此错误后，output 参数仍设置为正确的值。|  
|CORDBG_E_UNSUPPORTED_FORWARD_COMPAT|CLR 的版本高于此调试器支持的版本。 `pwszVersion`此错误后，output 参数仍设置为正确的值。|  
|E_NO_INTERFACE|`riidProcess`接口不可用。|  
|CORDBG_E_UNSUPPORTED_VERSION_STRUCT|`CLR_DEBUGGING_VERSION`结构没有可识别的值 `wStructVersion` 。 此时唯一接受的值为0。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
