---
title: ICorProfilerInfo::GetEventMask 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetEventMask
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetEventMask
helpviewer_keywords:
- GetEventMask method [.NET Framework profiling]
- ICorProfilerInfo::GetEventMask method [.NET Framework profiling]
ms.assetid: ec34cc13-45a3-4695-abc3-b3347d4e6fc2
topic_type:
- apiref
ms.openlocfilehash: 16bd8b09d5118171e669e9c428fb444384b5867d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722558"
---
# <a name="icorprofilerinfogeteventmask-method"></a>ICorProfilerInfo::GetEventMask 方法

获取探查器要从公共语言运行时 (CLR) 中接收其事件通知的当前事件类别。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetEventMask(  
    [out] DWORD *pdwEvents);  
```  
  
## <a name="parameters"></a>参数  

 `pdwEvents`  
 [out] 一个指向指定事件类别的 4 字节值的指针。 每个位都可控制不同的功能、行为或事件类型。 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)枚举中描述了这些位。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 应调用 [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) 方法，而不是此方法。 尽管此 `SetEventMask` 方法继续受支持，但 [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) 提供其他功能。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetEventMask2 方法](icorprofilerinfo5-geteventmask2-method.md)
- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
