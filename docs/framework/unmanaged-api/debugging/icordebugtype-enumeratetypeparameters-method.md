---
title: ICorDebugType::EnumerateTypeParameters 方法
ms.date: 03/30/2017
api_name:
- ICorDebugType.EnumerateTypeParameters
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType::EnumerateTypeParameters
helpviewer_keywords:
- EnumerateTypeParameters method, ICorDebugType interface [.NET Framework debugging]
- ICorDebugType::EnumerateTypeParameters method [.NET Framework debugging]
ms.assetid: 1ee1f6e6-1bd7-4ebb-83b8-ff9a08ca03de
topic_type:
- apiref
ms.openlocfilehash: 3717497ab6e72f0ce67f688813ee7264206e8c84
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727947"
---
# <a name="icordebugtypeenumeratetypeparameters-method"></a>ICorDebugType::EnumerateTypeParameters 方法

获取一个接口指针，该指针指向包含 <xref:System.Type> 此 ICorDebugType 引用的类的参数的 ICorDebugTypeEnum。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateTypeParameters (  
    [out] ICorDebugTypeEnum   **ppTyParEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppTyParEnum`  
 弄指向 `ICorDebugTypeEnum` 包含类型参数的的地址的指针。  
  
## <a name="remarks"></a>注解  

 `EnumerateTypeParameters`如果[ICorDebugType：： GetType](icordebugtype-gettype-method.md)返回的 CorElementType 值为 ELEMENT_TYPE_CLASS、ELEMENT_TYPE_VALUETYPE、ELEMENT_TYPE_ARRAY、ELEMENT_TYPE_SZARRAY、ELEMENT_TYPE_BYREF、ELEMENT_TYPE_PTR 或 ELEMENT_TYPE_FNPTR，则可以使用。 参数的数量及其顺序取决于类型：  
  
- ELEMENT_TYPE_CLASS 或 ELEMENT_TYPE_VALUETYPE：此方法返回的中包含的类型参数的数量 `ICorDebugTypeEnum` 将取决于相应类的形参数量。 例如，如果类型为 `class Dict<String,int32>` ，则 `EnumerateTypeParameters` 将返回， `ICorDebugTypeEnum` 其中包含 `String` 按顺序表示和的对象 `int32` 。  
  
- ELEMENT_TYPE_FNPTR：中包含的类型参数的数量 `ICorDebugTypeEnum` 将大于函数接受的参数的数目。 中包含的第一个类型参数 `ICorDebugTypeEnum` 是函数的返回类型，而后续类型参数是函数的参数。  
  
- ELEMENT_TYPE_ARRAY、ELEMENT_TYPE_SZARRAY、ELEMENT_TYPE_BYREF 或 ELEMENT_TYPE_PTR：将返回一个类型参数。 例如，如果类型是数组类型（如 `int32[]` ）， `EnumerateTypeParameters` 则将返回一个 `ICorDebugTypeEnum` ，它包含表示的对象 `int32` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
