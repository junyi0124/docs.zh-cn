---
title: ICorProfilerCallback4::ReJITCompilationFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITCompilationFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITCompilationFinished
helpviewer_keywords:
- ICorProfilerCallback4::ReJITCompilationFinished method [.NET Framework profiling]
- ReJITCompilationFinished method, ICorProfilerCallback4 interface [.NET Framework profiling]
ms.assetid: 3b5cff02-2005-44eb-a2bc-50214c4b0e1d
topic_type:
- apiref
ms.openlocfilehash: a6c2209433a652523fd8e3a7cc2db1272600e1bd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730261"
---
# <a name="icorprofilercallback4rejitcompilationfinished-method"></a>ICorProfilerCallback4::ReJITCompilationFinished 方法

通知探查器实时 (JIT) 编译器已经完成了对函数的重新编译。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReJITCompilationFinished(  
    [in] FunctionID functionId,    [in] ReJITID rejitId,  
    [in] HRESULT    hrStatus,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 中已重新编译的函数的 ID。  
  
 `rejitId`  
 [in] JIT 重新编译的函数的标识。  
  
 `hrStatus`  
 中一个值，该值指示 JIT 重新编译是否成功。  
  
 `fIsSafeToBlock`  
 [in] `true` 指示阻止可能会导致运行时等待调用线程从该回调返回; `false` 指示阻止操作不会影响运行时的操作。  
  
 的值 `true` 不会损害运行时，但会影响分析结果。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback4 接口](icorprofilercallback4-interface.md)
- [JITCompilationStarted 方法](icorprofilercallback-jitcompilationstarted-method.md)
- [ReJITCompilationStarted 方法](icorprofilercallback4-rejitcompilationstarted-method.md)
