---
title: ICorDebug::SetManagedHandler 方法
ms.date: 03/30/2017
api_name:
- ICorDebug.SetManagedHandler
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebug::SetManagedHandler
helpviewer_keywords:
- ICorDebug::SetManagedHandler method [.NET Framework debugging]
- SetManagedHandler method [.NET Framework debugging]
ms.assetid: d079131b-685b-4869-95be-826b88d28bd2
topic_type:
- apiref
ms.openlocfilehash: 97a4a464d3dfb7b333f44ac4206bd880fd171e16
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723410"
---
# <a name="icordebugsetmanagedhandler-method"></a>ICorDebug::SetManagedHandler 方法

指定托管事件的事件处理程序对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetManagedHandler (  
    [in] ICorDebugManagedCallback     *pCallback  
);  
```  
  
## <a name="parameters"></a>参数  

 `pCallback`  
 中指向 [ICorDebugManagedCallback](icordebugmanagedcallback-interface.md) 对象的指针，该对象为事件处理程序对象。  
  
## <a name="remarks"></a>注解  

 `SetManagedHandler` 必须在创建时调用。  
  
 如果 `ICorDebugManagedCallback` 实现未包含足够的接口来处理要调试的应用程序的调试事件，则将 `SetManagedHandler` 返回 E_NOINTERFACE 的 HRESULT。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebug 接口](icordebug-interface.md)
