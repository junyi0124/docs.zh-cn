---
title: ICorDebugModule::EnableClassLoadCallbacks 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule.EnableClassLoadCallbacks
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule::EnableClassLoadCallbacks
helpviewer_keywords:
- ICorDebugModule::EnableClassLoadCallbacks method [.NET Framework debugging]
- EnableClassLoadCallbacks method [.NET Framework debugging]
ms.assetid: 78dad5e4-8e2e-400f-bec3-92ff0205cd82
topic_type:
- apiref
ms.openlocfilehash: 1f6c6ae3b7cb45b049d0fb0d88bbf89121bccfd7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710410"
---
# <a name="icordebugmoduleenableclassloadcallbacks-method"></a>ICorDebugModule::EnableClassLoadCallbacks 方法

控制是否为此模块调用 [ICorDebugManagedCallback：： LoadClass](icordebugmanagedcallback-loadclass-method.md) 和 [ICorDebugManagedCallback：： UnloadClass](icordebugmanagedcallback-unloadclass-method.md) 回调。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnableClassLoadCallbacks(  
    [in] BOOL bClassLoadCallbacks  
);  
```  
  
## <a name="parameters"></a>参数  

 `bClassLoadCallbacks`  
 中将此值设置为 `true` ，以使公共语言运行时 (CLR) `ICorDebugManagedCallback::LoadClass` `ICorDebugManagedCallback::UnloadClass` 在其关联事件发生时调用和方法。  
  
 默认值为 `false` 非动态模块。 `true`对于动态模块，值始终为，且不能更改。  
  
## <a name="remarks"></a>注解  

 `ICorDebugManagedCallback::LoadClass`和 `ICorDebugManagedCallback::UnloadClass` 回调对于动态模块始终处于启用状态，并且不能禁用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
