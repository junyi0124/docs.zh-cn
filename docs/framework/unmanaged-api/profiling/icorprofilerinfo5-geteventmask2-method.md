---
title: ICorProfilerInfo5::GetEventMask2 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerInfo5.GetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: f854b68f-009c-4ffb-89cd-ca874d1c0fb7
topic_type:
- apiref
ms.openlocfilehash: 81509db178b0ab1a524dcc4b00f39264e87a220d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682778"
---
# <a name="icorprofilerinfo5geteventmask2-method"></a>ICorProfilerInfo5::GetEventMask2 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 获取探查器要从公共语言运行时 (CLR) 中接收通知的当前事件类别。  它提供 [ICorProfilerInfo：： GetEventMask](icorprofilerinfo-geteventmask-method.md) 方法未提供的功能。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetEventMask2(  
        [out] DWORD* pdwEventsLow,  
        [out] DWORD* pdwEventsHigh  
);  
```  
  
## <a name="parameters"></a>参数  

 `pdwEventsLow`  
 [out] 一个指向指定事件类别的 4 字节值的指针。 每个位都可控制不同的功能、行为或事件类型。 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)枚举中描述了这些位。  
  
 `pdwEventsHigh`  
 [out] 一个指向指定事件类别的 4 字节值的指针。  每个位都可控制不同的功能、行为或事件类型。 [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md)枚举中描述了这些位。  
  
## <a name="remarks"></a>注解  

 `GetEventMask2` 方法用于确定探查器已订阅了哪些回调。 通常，您 `pdwEventsLow` 需要对和 `pdwEventsHigh` 值以及要设置的任何新位执行逻辑 "或"，然后调用 [SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) 方法。  
  
 此方法是 [GetEventMask](icorprofilerinfo-geteventmask-method.md) 方法的建议替代方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo5 接口](icorprofilerinfo5-interface.md)
- [SetEventMask2 方法](icorprofilerinfo5-seteventmask2-method.md)
