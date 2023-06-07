---
title: ICorProfilerInfo8::GetDynamicFunctionInfo
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.GetDynamicFunctionInfo
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: eaf33f3b0de7a18e400cd16d29c046784e2e190f
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495315"
---
# <a name="icorprofilerinfo8getdynamicfunctioninfo-method"></a>ICorProfilerInfo8：： GetDynamicFunctionInfo 方法

检索有关动态方法的信息。

## <a name="syntax"></a>语法

```cpp
HRESULT GetDynamicFunctionInfo( [in]  FunctionID              functionId,
                                [out] ModuleID                *moduleId,
                                [out] PCCOR_SIGNATURE         *ppvSig,
                                [out] ULONG                   *pbSig,
                                [in]  ULONG                   cchName,
                                [out] ULONG                   *pcchName,
                                [out] WCHAR                   wszName[]);
```

## <a name="parameters"></a>参数

- `functionId`

  \[in] 要为其检索信息的函数的 ID。

- `moduleId`

  \[in] 一个指针，指向在其中定义函数父类的模块。

- `ppvSig`

  \[out] 指向函数的签名的指针。

- `pbSig`

  \[out] 指向函数签名的字节计数的指针。

- `cchName`

  \[in] 数组的最大大小 `wszName` 。

- `pcchName`

  \[out] 数组中的字符数 `wszName` 。

- `wszName`

  \[out] 作为 `WCHAR` 函数名称（如果存在）的数组。

## <a name="remarks"></a>注解

某些方法（如 IL 存根或 LCG）没有关联的元数据，可以使用[IMetaDataImport](../metadata/imetadataimport-interface.md)和[IMetaDataImport2](../metadata/imetadataimport2-interface.md) api 来检索这些元数据。 探查器可以通过指令指针或通过侦听[ICorProfilerCallback8：:D ynamicmethodjitcompilationstarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)来遇到此类方法。

此 API 可用于检索有关动态方法的信息，包括友好名称（如果可用）。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**头文件：** CorProf.idl、CorProf.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo8 接口](icorprofilerinfo8-interface.md)
