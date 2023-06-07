---
title: ICorDebugModule3::CreateReaderForInMemorySymbols 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule3.CreateReaderForInMemorySymbols
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule3::CreateReaderForInMemorySymbols
helpviewer_keywords:
- CreateReaderForInMemorySymbols method, ICorDebugModule3 interface [.NET Framework debugging]
- ICorDebugModule3::CreateReaderForInMemorySymbols method [.NET Framework debugging]
ms.assetid: af317171-d66d-4114-89eb-063554c74940
topic_type:
- apiref
ms.openlocfilehash: 44f4c59f95c28f9982d67875584e2f9803c0ed3b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709565"
---
# <a name="icordebugmodule3createreaderforinmemorysymbols-method"></a>ICorDebugModule3::CreateReaderForInMemorySymbols 方法

为动态模块创建调试符号读取器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateReaderForInMemorySymbols (  
      [in] REFIID riid,  
      [out][iid_is(riid)] void **    ppObj  
```  
  
## <a name="parameters"></a>参数  

 riid  
 中要返回的 COM 接口的 IID。 通常，这是一个 [ISymUnmanagedReader 接口](../diagnostics/isymunmanagedreader-interface.md)。  
  
 ppObj  
 弄指向返回接口的指针的指针。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功创建读取器。  
  
 CORDBG_E_MODULE_LOADED_FROM_DISK  
 该模块不是内存中或动态模块。  
  
 CORDBG_E_SYMBOLS_NOT_AVAILABLE  
 符号尚未由应用程序提供或尚未提供。  
  
 E_FAIL（或其他 E_ 返回代码）  
 无法创建读取器。  
  
## <a name="remarks"></a>注解  

 此方法还可用于为内存中 (非动态) 模块创建符号读取器对象，但仅在符号首次可用之后 ([UpdateModuleSymbols 方法](icordebugmanagedcallback-updatemodulesymbols-method.md) 回调) 指示。  
  
 此方法会在每次调用时返回一个新的读取器实例 (如 [CComPtrBase：： CoCreateInstance](/cpp/atl/reference/ccomptrbase-class#cocreateinstance)) 。 因此，只有当基础数据可能已更改时，调试器才应缓存结果，并请求新实例 (也就是说，当) 接收到 [LoadClass 方法](icordebugmanagedcallback-loadclass-method.md) 回调时。  
  
 在加载第一个类型之前，动态模块没有可用的任何符号 (如 [LoadClass 方法](icordebugmanagedcallback-loadclass-method.md) 回调) 所示。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 4.5、4、3.5 SP1  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)
- [ICorDebug 接口](icordebug-interface.md)

- [调试接口](debugging-interfaces.md)
