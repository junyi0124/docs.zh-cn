---
title: ICorDebugProcess5::GetGCHeapInformation 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.GetGCHeapInformation
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::GetGCHeapInformation
helpviewer_keywords:
- ICorDebugProcess5::GetGCHeapInformation method [.NET Framework debugging]
- GetGCHeapInformation method, ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: b9538ceb-230a-4079-9cb2-903dbf5c1848
topic_type:
- apiref
ms.openlocfilehash: 43054a71445193bae7a74e0ed6785cccd1d02dc2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95670987"
---
# <a name="icordebugprocess5getgcheapinformation-method"></a>ICorDebugProcess5::GetGCHeapInformation 方法

提供有关垃圾回收堆的常规信息，包括当前是否可枚举。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetGCHeapInformation(  
    [out] COR_HEAPINFO *pHeapInfo  
);  
```  
  
## <a name="parameters"></a>参数  

 `pHeapInfo`  
 弄一个指向 [COR_HEAPINFO](cor-heapinfo-structure.md) 值的指针，该值提供有关垃圾回收堆的常规信息。  
  
## <a name="remarks"></a>注解  

 `ICorDebugProcess5::GetGCHeapInformation`必须先调用方法，然后才能枚举堆或单独的堆区域，以确保进程中的垃圾回收结构当前有效。 当集合正在进行时，无法遍历垃圾回收堆。 否则，枚举可能会捕获无效的垃圾回收结构。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugProcess5 接口](icordebugprocess5-interface.md)
- [调试接口](debugging-interfaces.md)
