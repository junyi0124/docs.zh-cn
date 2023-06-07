---
title: ICorDebugEval2::NewParameterizedArray 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval2.NewParameterizedArray
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval2::NewParameterizedArray
helpviewer_keywords:
- ICorDebugEval2::NewParameterizedArray method [.NET Framework debugging]
- NewParameterizedArray method [.NET Framework debugging]
ms.assetid: 45efb8ba-c4de-4109-945f-e734d376b43c
topic_type:
- apiref
ms.openlocfilehash: 14274932461fa7a5278c9a09b421f50be098cb91
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729657"
---
# <a name="icordebugeval2newparameterizedarray-method"></a>ICorDebugEval2::NewParameterizedArray 方法

分配指定元素类型和维度的新数组。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NewParameterizedArray(  
    [in] ICorDebugType          *pElementType,  
    [in] ULONG32                rank,  
    [in, size_is(rank)] ULONG32 dims[],  
    [in, size_is(rank)] ULONG32 lowBounds[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `pElementType`  
 中指向 ICorDebugType 对象的指针，该对象表示存储在数组中的元素的类型。  
  
 `rank`  
 中数组的维数。 在 .NET Framework 版本2.0 中，此值必须为1。  
  
 `dims`  
 中数组每个维度的大小（以字节为单位）。  
  
 `lowBounds`  
 [in] 可选。 数组的每个维度的下限。 如果省略此值，则假定每个维度的下限为零。  
  
## <a name="remarks"></a>注解  

 数组的元素可以是泛型类型的实例。 始终在当前运行线程的应用程序域中创建数组。 在 .NET Framework 2.0 中，的值 `rank` 必须为1。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
