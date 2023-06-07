---
title: ICorProfilerInfo10::SuspendRuntime
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo10.SuspendRuntime
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 121ed0401628193f6e2fe632a124c08aad7bd1b4
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90551434"
---
# <a name="icorprofilerinfo10suspendruntime-method"></a>ICorProfilerInfo10：： SuspendRuntime 方法

挂起运行时，而不执行 GC。

## <a name="syntax"></a>语法

```cpp
HRESULT SuspendRuntime();
```

## <a name="requirements"></a>要求

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。

**头文件：** CorProf.idl、CorProf.h

**库：** CorGuids.lib

**.Net 版本：**[!INCLUDE[net_core_22](../../../../includes/net-core-30-md.md)]

## <a name="see-also"></a>请参阅

- [ICorProfilerInfo10 接口](icorprofilerinfo10-interface.md)
