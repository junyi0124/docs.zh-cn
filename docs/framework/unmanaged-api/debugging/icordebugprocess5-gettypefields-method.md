---
title: ICorDebugProcess5::GetTypeFields 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5.GetTypeFields
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5::GetTypeFields
helpviewer_keywords:
- GetTypeFields method, ICorDebugProcess5 interface [.NET Framework debugging]
- ICorDebugProcess5::GetTypeFields method [.NET Framework debugging]
ms.assetid: 6a0ad3ee-dacb-47e9-abae-4536bcc4804b
topic_type:
- apiref
ms.openlocfilehash: e4eba37487ca2ee0a88caf5a59f86949a6521e40
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95670935"
---
# <a name="icordebugprocess5gettypefields-method"></a>ICorDebugProcess5::GetTypeFields 方法

提供有关属于类型的字段的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetTypeFields(  
    [in] COR_TYPEID id,  
    [in] ULONG32 celt,  
    [out] COR_FIELD fields[],
    [out] ULONG32 *pceltNeeded  
);  
```  
  
## <a name="parameters"></a>参数  

 `id`  
 中检索其字段信息的类型的标识符。  
  
 `celt`  
 中要检索其字段信息的 [COR_FIELD](cor-field-structure.md) 对象的数量。  
  
 `fields`  
 弄 [COR_FIELD](cor-field-structure.md) 对象的数组，这些对象提供有关属于类型的字段的信息。  
  
 `pceltNeeded`  
 弄一个指针，指向中包含的 [COR_FIELD](cor-field-structure.md) 对象的数量 `fields` 。  
  
## <a name="remarks"></a>注解  

 `celt`参数，该参数指定方法用来填充字段信息的字段数量 `fields` ，应与字段的值相对应 `COR_TYPE_LAYOUT::numFields` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugProcess5 接口](icordebugprocess5-interface.md)
- [调试接口](debugging-interfaces.md)
