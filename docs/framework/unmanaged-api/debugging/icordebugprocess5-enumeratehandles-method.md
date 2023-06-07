---
title: ICorDebugProcess5::EnumerateHandles 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.EnumerateHandles
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::EnumerateHandles
helpviewer_keywords:
- EnumerateHandles method, ICorDebugProcess5 interface [.NET Framework debugging]
- ICorDebugProcess5::EnumerateHandles method [.NET Framework debugging]
ms.assetid: 7d7fa796-0dc6-4ee8-9d56-40166246d91d
topic_type:
- apiref
ms.openlocfilehash: 607847180cca039d4c71f26e446a17a14dc2fb9e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724333"
---
# <a name="icordebugprocess5enumeratehandles-method"></a>ICorDebugProcess5::EnumerateHandles 方法

获取进程中的对象句柄的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateHandles(     [in] CorGCReferenceType types,  
    [out] ICorDebugGCReferenceEnum **ppEnum);  
```  
  
## <a name="parameters"></a>参数  

 `types`  
 中 [CorGCReferenceType](corgcreferencetype-enumeration.md) 值的按位组合，用于指定要包括在集合中的句柄的类型。  
  
 `ppENum`  
 弄一个指针，指向 [ICorDebugGCReferenceEnum](icordebuggcreferenceenum-interface.md) 的地址，该地址是要进行垃圾回收的对象的枚举器。  
  
## <a name="remarks"></a>注解  

 `EnumerateHandles` 是支持对句柄表的检查的 helper 函数。 它类似于 [ICorDebugProcess5：： EnumerateGCReferences](icordebugprocess5-enumerategcreferences-method.md) 方法，不同之处在于，它不是用所有要进行垃圾回收的对象填充 [ICorDebugGCReferenceEnum](icordebuggcreferenceenum-interface.md) 集合，只包括具有句柄表中的句柄的对象。  
  
 `types`参数指定要包括在集合中的句柄类型。 `types` 可以是 [CorGCReferenceType](corgcreferencetype-enumeration.md) 枚举的以下三个成员之一：  
  
- `CorHandleStrongOnly` 仅)  (句柄。  
  
- `CorHandleWeakOnly` 仅)  (句柄到弱引用。  
  
- `CorHandleAll` (所有) 的句柄。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
