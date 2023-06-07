---
title: ICorProfilerInfo8::IsFunctionDynamic
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.IsFunctionDynamic
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: c88279d361ea78a2e910c4621e92c500902d9124
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84495120"
---
# <a name="icorprofilerinfo8isfunctiondynamic-method"></a>ICorProfilerInfo8：： IsFunctionDynamic 方法

确定函数是否没有关联的元数据。

## <a name="syntax"></a>语法

```cpp
HRESULT IsFunctionDynamic( [in]  FunctionID  functionId,
                           [out] BOOL        *isDynamic);
```

## <a name="parameters"></a>参数

- `functionId`

  \[in] `FunctionID` 标识相关函数的。

- `isDynamic`

  \[out] 指向的指针 `BOOL` ，它将包含一个值，该值指示该函数是否没有元数据。

## <a name="remarks"></a>注解

如果函数没有元数据，则将其视为动态函数。 某些方法（如 IL 存根或 LCG 方法）没有关联的元数据，可以使用 IMetaDataImport Api 来检索这些元数据。 探查器可以通过指令指针或通过侦听 ICorProfilerCallback 来完成这些方法[：:D ynamicmethodjitcompilationstarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**头文件：** CorProf.idl、CorProf.h

**库：** CorGuids.lib

**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo8 接口](icorprofilerinfo8-interface.md)
