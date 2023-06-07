---
title: ICorDebugVariableHome：： GetArgumentIndex 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetArgumentIndex
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetArgumentIndex
helpviewer_keywords:
- ICorDebugVariableHome::GetArgumentIndex method [.NET Framework debugging]
- GetArgumentIndex method, ICorDebugVariableHome interface [.NET Framework debugging]
ms.assetid: e86fcc72-388d-4009-ab21-8f9c3323e9a3
topic_type:
- apiref
ms.openlocfilehash: 27a676fd1d2d7903943e44f8a7201b88af4fba89
ms.sourcegitcommit: 046a9c22487551360e20ec39fc21eef99820a254
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83396998"
---
# <a name="icordebugvariablehomegetargumentindex-method"></a>ICorDebugVariableHome：： GetArgumentIndex 方法

获取函数参数的索引。

## <a name="syntax"></a>语法

```cpp
HRESULT GetArgumentIndex(
    [out] ULONG32* pArgumentIndex
);
```

## <a name="parameters"></a>参数

`pArgumentIndex`\
弄指向参数索引的指针。

## <a name="return-value"></a>返回值

方法返回以下值。

|“值”|说明|
|-----------|-----------------|
|`S_OK`|方法调用返回有效的参数索引。|
|`E_FAIL`|当前[ICorDebugVariableHome](icordebugvariablehome-interface.md)实例表示局部变量。|

## <a name="remarks"></a>备注

参数索引可用于检索此参数的元数据。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]

## <a name="see-also"></a>请参阅

- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
