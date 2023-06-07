---
title: ICorProfilerInfo5::SetEventMask2 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- IcorProfilerInfo5.SetEventMask2
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 05dbbe2b-049c-4a60-be69-2ad7a949405e
topic_type:
- apiref
ms.openlocfilehash: 75e2bfc8dfae4d0cd453eba0697d6ee2f0da7133
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733784"
---
# <a name="icorprofilerinfo5seteventmask2-method"></a>ICorProfilerInfo5::SetEventMask2 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 设置一个值，用于指定探查器需要从公共语言运行时 (CLR) 接收其事件通知的事件的类型。 它提供的功能比 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法更多。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT SetEventMask2(        [in] DWORD dwEventsLow,        [in] DWORD dwEventsHigh  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwEventsLow`  
 [in] 一个指定事件类别的 4 字节的值。 每个位都可控制不同的功能、行为或事件类型。 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md)枚举中描述了这些位。  
  
 `dwEventsHigh`  
 [in] 一个指定事件类别的 4 字节的值。  每个位都可控制不同的功能、行为或事件类型。 [COR_PRF_HIGH_MONITOR](cor-prf-high-monitor-enumeration.md)枚举中描述了这些位。  
  
## <a name="remarks"></a>注解  

 `SetEventMask2` 方法用于设置探查器订阅的回调。 通常，你可以调用 [GetEventMask2](icorprofilerinfo5-geteventmask2-method.md) 方法来确定设置了哪些位，对其 `pdwEventsLow` 和 `pdwEventsHigh` 值以及要设置的任何新位执行逻辑 "或"，然后调用 `SetEventMask2` 方法。  
  
 此方法是 [SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法的建议替代方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo5 接口](icorprofilerinfo5-interface.md)
- [GetEventMask2 方法](icorprofilerinfo5-geteventmask2-method.md)
