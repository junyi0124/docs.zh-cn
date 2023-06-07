---
title: ICorProfilerInfo3::SetFunctionIDMapper2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.SetFunctionIDMapper2 Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::SetFunctionIDMapper2
helpviewer_keywords:
- SetFunctionIDMapper2 method [.NET Framework profiling]
- ICorProfilerInfo3::SetFunctionIDMapper2 method [.NET Framework profiling]
ms.assetid: 8cdb1188-952a-4ba8-9f05-bfebc18cdd29
topic_type:
- apiref
ms.openlocfilehash: 26c26cf204f1a2743f46cfcfdfadbf2c3e3df38e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721564"
---
# <a name="icorprofilerinfo3setfunctionidmapper2-method"></a>ICorProfilerInfo3::SetFunctionIDMapper2 方法

指定将调用以将 `FunctionID` 值映射至替换值（传递至探查器的输入/退出挂钩）的探查器实现函数。 此方法将 [ICorProfilerInfo：： SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) 方法与其他数据参数进行扩展，探查器可能会使用该参数来消除运行时之间的歧义。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetFunctionIDMapper2(  
       [in] FunctionIDMapper2 *pFunc,  
       [in] void *clientData);  
```  
  
## <a name="parameters"></a>参数  

 `pFunc`  
 中指向 [FunctionIDMapper2](functionidmapper2-function.md) 实现的指针，将调用该指针以将 `FunctionID` 值映射到其可选值。  
  
 `clientData`  
 中传递给当前运行时所做的每个 [FunctionIDMapper2](functionidmapper2-function.md) 函数调用的指针。 探查器可使用此信息来消除运行时之间的歧义。  
  
## <a name="return-value"></a>返回值  
  
## <a name="remarks"></a>注解  

 FunctionID 值的替代项将传递到探查器的函数入口/出口挂钩 ([FunctionEnter3](functionenter3-function.md)、 [FunctionLeave3](functionleave3-function.md)和 [FunctionTailcall3](functiontailcall3-function.md);、 [FunctionEnter3WithInfo](functionenter3withinfo-function.md)、 [FunctionLeave3WithInfo](functionleave3withinfo-function.md)和 [FunctionTailcall3WithInfo](functiontailcall3withinfo-function.md)) 由 [SetEnterLeaveFunctionHooks3](icorprofilerinfo3-setenterleavefunctionhooks3-method.md) 或 [SetEnterLeaveFunctionHooks3WithInfo](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) 方法指定。  
  
 此 `FunctionIDMapper2` 方法只能设置一次; 建议在 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md) 回调中进行设置。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md)
- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
