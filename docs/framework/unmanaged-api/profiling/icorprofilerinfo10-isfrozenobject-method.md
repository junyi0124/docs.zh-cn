---
title: ICorProfilerInfo10::IsFrozenObject
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo10.IsFrozenObject
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 3755260b885768de6b5b2d6342c0ad590a95caff
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90548665"
---
# <a name="icorprofilerinfo10isfrozenobject-method"></a>ICorProfilerInfo10：： IsFrozenObject 方法

给定 ObjectID，确定对象是否位于只读段。

## <a name="syntax"></a>语法

```cpp
HRESULT IsFrozenObject( [in]  ObjectID objectId,
                        [out] BOOL *pbFrozen);
```

## <a name="parameters"></a>参数

- `objectId`

  \[in] 要检查的对象。

- `pbFrozen`

  \[out] 一个 `BOOL` 值，该值指示对象是否位于只读段中。

## <a name="requirements"></a>要求

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。

**头文件：** CorProf.idl、CorProf.h

**库：** CorGuids.lib

**.Net 版本：**[!INCLUDE[net_core_22](../../../../includes/net-core-30-md.md)]

## <a name="see-also"></a>请参阅

- [ICorProfilerInfo10 接口](icorprofilerinfo10-interface.md)
