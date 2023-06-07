---
title: ICorDebugManagedCallback::UpdateModuleSymbols 方法
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback.UpdateModuleSymbols
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback::UpdateModuleSymbols
helpviewer_keywords:
- UpdateModuleSymbols method [.NET Framework debugging]
- ICorDebugManagedCallback::UpdateModuleSymbols method [.NET Framework debugging]
ms.assetid: 0863f644-58e8-45a0-b0c3-a28e99b20938
topic_type:
- apiref
ms.openlocfilehash: 1615d00a9a25cd2f4aa7d9b84de54b5e7670a3fc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730563"
---
# <a name="icordebugmanagedcallbackupdatemodulesymbols-method"></a>ICorDebugManagedCallback::UpdateModuleSymbols 方法

通知调试器公共语言运行时模块的符号已发生更改。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT UpdateModuleSymbols (  
    [in] ICorDebugAppDomain *pAppDomain,  
    [in] ICorDebugModule    *pModule,  
    [in] IStream            *pSymbolStream  
);  
```  
  
## <a name="parameters"></a>参数  

 `pAppDomain`  
 中指向 ICorDebugAppDomain 对象的指针，该对象表示包含已更改符号的模块的应用程序域。  
  
 `pModule`  
 中指向 ICorDebugModule 对象的指针，该对象表示符号已更改的模块。  
  
 `pSymbolStream`  
 中指向 Win32 COM `IStream` 对象的指针，该对象包含修改后的符号。  
  
## <a name="remarks"></a>注解  

 此方法通过调用 [ISymUnmanagedReader：： UpdateSymbolStore](../diagnostics/isymunmanagedreader-updatesymbolstore-method.md) 或 [ISymUnmanagedReader：： ReplaceSymbolStore](../diagnostics/isymunmanagedreader-replacesymbolstore-method.md)，提供更新模块的符号的调试器视图的机会。  
  
 对于同一个模块，此回调可能发生多次。  
  
 调试器应尝试绑定未绑定的源级断点。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)
