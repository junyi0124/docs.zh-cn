---
title: ICorDebugThread4::GetBlockingObjects 方法
ms.date: 03/30/2017
api_name:
- ICorDebugThread4.GetBlockingObjects Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugThread4::GetBlockingObjects
helpviewer_keywords:
- GetBlockingObjects method [.NET Framework debugging]
- ICorDebugThread4::GetBlockingObjects method [.NET Framework debugging]
ms.assetid: a7e6c54e-7be9-4e52-bbb4-95f52458e8e4
topic_type:
- apiref
ms.openlocfilehash: eb8692aebe7b702b5778b3f13e496d81dcd45784
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678531"
---
# <a name="icordebugthread4getblockingobjects-method"></a>ICorDebugThread4::GetBlockingObjects 方法

提供 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构的有序枚举，这些结构提供线程阻塞信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetBlockingObjects (  
    [out] ICorDebugBlockingObjectEnum **ppBlockingObjectEnum  
```  
  
## <a name="parameters"></a>参数  

 `ppBlockingObjectEnum`  
 弄指向 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构的有序枚举的指针。  
  
## <a name="remarks"></a>注解  

 返回的枚举中的第一个元素对应于阻塞线程的第一个结构。 第二个元素对应于在第一次阻塞时 (APC) 运行异步过程调用时遇到的阻塞项，依此类推。  
  
 枚举仅在当前已同步状态的持续时间内有效。  
  
 当调试对象处于已同步状态时，必须调用此方法。  
  
 如果不是 `ppBlockingObjectEnum` 有效的指针，则结果是不确定的。  
  
 如果某个线程被阻止并且无法确定该错误，则该方法将返回一个指示失败的 HRESULT;否则，它将返回 S_OK。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugThread4 接口](icordebugthread4-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
