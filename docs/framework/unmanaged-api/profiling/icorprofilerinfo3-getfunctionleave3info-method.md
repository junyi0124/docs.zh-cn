---
title: ICorProfilerInfo3::GetFunctionLeave3Info 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.GetFunctionLeave3Info Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::GetFunctionLeave3Info
helpviewer_keywords:
- GetFunctionLeave3Info method [.NET Framework profiling]
- ICorProfilerInfo3::GetFunctionLeave3Info method [.NET Framework profiling]
ms.assetid: df7083d2-fd43-44c7-9ce5-912c25cef0ff
topic_type:
- apiref
ms.openlocfilehash: f365a95b0859f4f97dab96ec85af6d7dfb96d8e5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721608"
---
# <a name="icorprofilerinfo3getfunctionleave3info-method"></a>ICorProfilerInfo3::GetFunctionLeave3Info 方法

提供由 [FunctionLeave3WithInfo 函数](functionleave3withinfo-function.md) 函数向探查器报告的函数的堆栈帧和返回值。 仅在 `FunctionLeave3WithInfo` 回调时可调用此方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFunctionLeave3Info(  
            [in]  FunctionID functionId,  
            [in]  COR_PRF_ELT_INFO eltInfo,  
            [out] COR_PRF_FRAME_INFO *pFrameInfo,  
            [out] COR_PRF_FUNCTION_ARGUMENT_RANGE *pRetvalRange);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 中 `FunctionID` 正在返回的函数的。  
  
 `eltInfo`  
 [in] 表示有关给定堆栈帧的信息的不透明的句柄。 探查器应提供 `eltInfo` [FunctionLeave3WithInfo](functionleave3withinfo-function.md) 函数为探查器提供的相同的。  
  
 `pFrameInfo`  
 [out] 表示有关给定堆栈帧的泛型信息的不透明的句柄。 此句柄仅在探查器调用 `GetFunctionLeave3Info` 方法的 `FunctionLeave3WithInfo` 回调时有效。  
  
 `pRetvalRange`  
 弄指向 [COR_PRF_FUNCTION_ARGUMENT_RANGE](cor-prf-function-argument-range-structure.md) 结构的指针，该结构包含从函数返回的值。 若要访问返回值信息， `COR_PRF_ENABLE_FUNCTION_RETVAL` 必须设置标志。 探查器可以使用 [ICorProfilerInfo：： SetEventMask 方法](icorprofilerinfo-seteventmask-method.md) 来设置事件标志。  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [FunctionEnter3WithInfo](functionenter3withinfo-function.md)
- [FunctionLeave3WithInfo](functionleave3withinfo-function.md)
- [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)
- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
