---
title: IMethodMalloc::Alloc 方法
ms.date: 03/30/2017
api_name:
- IMethodMalloc.Alloc
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- IMethodMalloc::Alloc
helpviewer_keywords:
- IMethodMalloc::Alloc method [.NET Framework profiling]
- Alloc method, IMethodMalloc interface [.NET Framework profiling]
ms.assetid: 8653bd4c-2290-43d2-a3e1-cbbd50033f4f
topic_type:
- apiref
ms.openlocfilehash: 58809f12e4dd4419b754caafc3f8b883b8bc5089
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721161"
---
# <a name="imethodmallocalloc-method"></a>IMethodMalloc::Alloc 方法

尝试将指定数量的内存分配给新的 Microsoft 中间语言 (MSIL) 函数体。

## <a name="syntax"></a>语法

```cpp
PVOID Alloc (
    [in] ULONG   cb
);
```

## <a name="parameters"></a>参数

`cb`\
中要为方法主体分配的字节数。

## <a name="remarks"></a>注解

 分配的内存将以大于与此分配器关联的模块的基址的地址开始。 换言之，每个分配器都是为特定模块创建的，并将尝试从其基址的正偏移量处分配内存。 如果 `Alloc` 无法在大于模块基址的地址分配请求的字节数，则将返回 E_OUTOFMEMORY，而不考虑实际可用的内存空间量。

 `Alloc`应将方法与[ICorProfilerInfo：： SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md)方法结合使用。

## <a name="requirements"></a>要求

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。

 **头文件：** CorProf.idl、CorProf.h

 **库：** CorGuids.lib

 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]

## <a name="see-also"></a>另请参阅

- [IMethodMalloc 接口](imethodmalloc-interface.md)
