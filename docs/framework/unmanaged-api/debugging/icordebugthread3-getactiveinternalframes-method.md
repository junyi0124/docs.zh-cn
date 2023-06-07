---
title: ICorDebugThread3::GetActiveInternalFrames 方法
ms.date: 03/30/2017
api_name:
- ICorDebugThread3.GetActiveInternalFrames Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread3::GetActiveInternalFrames
helpviewer_keywords:
- ICorDebugThread3::GetActiveInternalFrames method [.NET Framework debugging]
- GetActiveInternalFrames method [.NET Framework debugging]
ms.assetid: d69796b4-5b6d-457c-85f6-2cf42e8a8773
topic_type:
- apiref
ms.openlocfilehash: 2ca3bf94298b45e404c930ffe52e101085ee482d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726205"
---
# <a name="icordebugthread3getactiveinternalframes-method"></a>ICorDebugThread3::GetActiveInternalFrames 方法

返回堆栈 ([ICorDebugInternalFrame2](icordebuginternalframe2-interface.md) 对象) 内部帧的数组。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetActiveInternalFrames  
      (  
      [in] ULONG32 cInternalFrames,  
      [out] ULONG32 *pcInternalFrames,  
      [in, out,size_is(cInternalFrames), length_is(*pcInternalFrames)]  
            ICorDebugInternalFrame2 * ppInternalFrames[]  
      );  
```  
  
## <a name="parameters"></a>参数  

 `cInternalFrames`  
 中中所需的内部帧的数目 `ppInternalFrames` 。  
  
 `pcInternalFrames`  
 弄指向的指针 `ULONG32` ，该指针包含堆栈上的内部帧的数目。  
  
 `ppInternalFrames`  
 [in，out]一个指针，指向堆栈上内部帧的数组的地址。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|已成功创建 [ICorDebugInternalFrame2](icordebuginternalframe2-interface.md) 对象。|  
|E_INVALIDARG|`cInternalFrames` 不为零，并且 `ppInternalFrames` 为 `null` 或 `pcInternalFrames` 为 `null` 。|  
|HRESULT_FROM_WIN32(ERROR_INSUFFICIENT_BUFFER)|`ppInternalFrames` 小于内部帧的计数。|  
  
## <a name="exceptions"></a>例外  
  
## <a name="remarks"></a>备注  

 内部帧是由运行时推送到堆栈上的数据结构，用于存储临时数据。  
  
 第一次调用时 `GetActiveInternalFrames` ，应将 `cInternalFrames` 参数设置为 0 (零) ，将参数设置 `ppInternalFrames` 为 null。 `GetActiveInternalFrames`第一次返回时， `pcInternalFrames` 包含堆栈上内部帧的计数。  
  
 `GetActiveInternalFrames` 然后，应再次调用。 应在参数中传递正确的计数 (`pcInternalFrames`) `cInternalFrames` ，并在中指定一个指向适当大小的数组的指针 `ppInternalFrames` 。  
  
 使用 [ICorDebugStackWalk：： GetFrame](icordebugthread3-getactiveinternalframes-method.md) 方法返回实际的堆栈帧。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
