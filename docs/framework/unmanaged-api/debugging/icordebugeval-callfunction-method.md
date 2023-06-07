---
title: ICorDebugEval::CallFunction 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval.CallFunction
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::CallFunction
helpviewer_keywords:
- ICorDebugEval::CallFunction method [.NET Framework debugging]
- CallFunction method [.NET Framework debugging]
ms.assetid: 7f470c5c-e1c0-4d8d-aad8-830f113ae751
topic_type:
- apiref
ms.openlocfilehash: 1cf0080945ad78565fae3fedb454ceba7825cb4a
ms.sourcegitcommit: fff146ba3fd1762c8c432d95c8b877825ae536fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82976234"
---
# <a name="icordebugevalcallfunction-method"></a>ICorDebugEval::CallFunction 方法

设置对指定函数的调用。

此方法在 .NET Framework 版本2.0 中已过时。 改[为使用 ICorDebugEval2：： CallParameterizedFunction](icordebugeval2-callparameterizedfunction-method.md) 。

## <a name="syntax"></a>语法

```cpp
HRESULT CallFunction (
    [in] ICorDebugFunction  *pFunction,
    [in] ULONG32            nArgs,
    [in, size_is(nArgs)] ICorDebugValue *ppArgs[]
);
```

## <a name="parameters"></a>参数

`pFunction`\
中指向 ICorDebugFunction 对象的指针，该对象指定要调用的函数。

`nArgs`\
中函数的参数数目。

`ppArgs`\
中指针的数组，其中每个都指向一个 ICorDebugValue 对象，该对象指定要传递给函数的参数。

## <a name="remarks"></a>备注

如果该函数是虚拟的`CallFunction` ，将执行虚拟调度。 如果该函数在不同的应用程序域中，只要所有参数也在该应用程序域中，就会发生转换。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头**：CorDebug.idl、CorDebug.h

**库：** CorGuids.lib

**.NET Framework 版本：** 1.1、1。0

## <a name="see-also"></a>另请参阅

- [CallParameterizedFunction 方法](icordebugeval2-callparameterizedfunction-method.md)
