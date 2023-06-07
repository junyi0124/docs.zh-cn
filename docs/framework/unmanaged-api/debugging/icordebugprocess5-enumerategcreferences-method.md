---
title: ICorDebugProcess5::EnumerateGCReferences 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.EnumerateGCReferences
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::EnumerateGCReferences
helpviewer_keywords:
- EnumerateGCReferences method, ICorDebugProcess5 interface [.NET Framework debugging]
- ICorDebugProcess5::EnumerateGCReferences method [.NET Framework debugging]
ms.assetid: 86c397c3-81d8-463e-a248-3cbe06c44d9d
topic_type:
- apiref
ms.openlocfilehash: 0f2f5acfc6a23398b15af3a63345050eb0dfd5b4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687185"
---
# <a name="icordebugprocess5enumerategcreferences-method"></a>ICorDebugProcess5::EnumerateGCReferences 方法

获取一个枚举器，该枚举器用于进程中要进行垃圾回收的所有对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateGCReferences(  
    [in] Bool enumerateWeakReferences,
    [out] ICorDebugGCReferenceEnum **ppEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `enumerateWeakReferences`  
 中指示是否也要枚举弱引用的布尔值。 如果 `enumerateWeakReferences` 为 `true` ，则 `ppEnum` 枚举器将同时包含强引用和弱引用。 如果 `enumerateWeakReferences` 为 `false` ，则枚举器仅包括强引用。  
  
 `ppEnum`  
 弄一个指针，指向 [ICorDebugGCReferenceEnum](icordebuggcreferenceenum-interface.md) 的地址，该地址是要进行垃圾回收的对象的枚举器。  
  
## <a name="remarks"></a>注解  

 此方法提供了一种方法，用于确定进程中任何托管对象的完整根链，并可用于确定对象仍处于活动状态的原因。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugProcess5 接口](icordebugprocess5-interface.md)
- [调试接口](debugging-interfaces.md)
