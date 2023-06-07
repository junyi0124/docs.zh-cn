---
title: ICorDebugModule::IsDynamic 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule.IsDynamic
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule::IsDynamic
helpviewer_keywords:
- IsDynamic method [.NET Framework debugging]
- ICorDebugModule::IsDynamic method [.NET Framework debugging]
ms.assetid: 5eefe716-5025-4a4c-970c-c823cdc7bb87
topic_type:
- apiref
ms.openlocfilehash: 98c01993a85ed07d961902d8a098a96df4702c76
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709825"
---
# <a name="icordebugmoduleisdynamic-method"></a>ICorDebugModule::IsDynamic 方法

获取一个值，该值指示此模块是否为动态模块。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsDynamic(  
    [out] BOOL *pDynamic  
);  
```  
  
## <a name="parameters"></a>参数  

 `pDynamic`  
 [out] `true` 如果此模块是动态的，则为;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 即使在模块加载后，动态模块也可以添加新类和删除现有类。 添加或删除类时， [ICorDebugManagedCallback：： LoadClass](icordebugmanagedcallback-loadclass-method.md) 和 [ICorDebugManagedCallback：： UnloadClass](icordebugmanagedcallback-unloadclass-method.md) 回调会通知调试器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
