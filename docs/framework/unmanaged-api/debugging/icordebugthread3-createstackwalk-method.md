---
title: ICorDebugThread3::CreateStackWalk 方法
ms.date: 03/30/2017
api_name:
- ICorDebugThread3.CreateStackWalk Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread3::CreateStackWalk
helpviewer_keywords:
- CreateStackWalk method [.NET Framework debugging]
- ICorDebugThread3::CreateStackWalk method [.NET Framework debugging]
ms.assetid: c55e35d9-f9aa-4268-94b5-dce44c61acf2
topic_type:
- apiref
ms.openlocfilehash: fb9d07fffd2ec98225ce60b211f525f8dafd9725
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95691062"
---
# <a name="icordebugthread3createstackwalk-method"></a>ICorDebugThread3::CreateStackWalk 方法

为要展开其堆栈的线程创建 [ICorDebugStackWalk](icordebugstackwalk-interface.md) 对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateStackWalk([out] ICorDebugStackWalk **ppStackWalk);  
```  
  
## <a name="parameters"></a>参数  

 `ppStackWalk`  
 弄指向要展开其堆栈的线程的 [ICorDebugStackWalk](icordebugstackwalk-interface.md) 对象地址的指针。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|已 `ICorDebugStackWalk` 成功创建对象。|  
|E_FAIL|`ICorDebugStackWalk`未创建对象。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 如果该 `CreateStackWalk` 方法成功，则返回的 `ICorDebugStackWalk` 对象的上下文将设置为线程的当前上下文。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
