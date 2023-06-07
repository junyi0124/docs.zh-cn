---
title: ICorProfilerCallback::JITCompilationStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCompilationStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCompilationStarted
helpviewer_keywords:
- JITCompilationStarted method [.NET Framework profiling]
- ICorProfilerCallback::JITCompilationStarted method [.NET Framework profiling]
ms.assetid: 31782b36-d311-4518-8f45-25f65385af5b
topic_type:
- apiref
ms.openlocfilehash: 7ce100a68a3e2b8963ed14bbf044fa9ba11d629f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725508"
---
# <a name="icorprofilercallbackjitcompilationstarted-method"></a>ICorProfilerCallback::JITCompilationStarted 方法

通知探查器实时 (JIT) 编译器已开始编译函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT JITCompilationStarted(  
    [in] FunctionID functionId,  
    [in] BOOL       fIsSafeToBlock);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 中要开始编译的函数的 ID。  
  
 `fIsSafeToBlock`  
 中指示探查器是否会影响运行时的操作的值。 `true`如果阻止可能会导致运行时等待调用线程从此回调返回，则该值为; 否则为 `false` 。  
  
 尽管的值 `true` 不会损害运行时，但它可能会使分析结果变形。  
  
## <a name="remarks"></a>注解  

 `JITCompilationStarted`由于运行时处理类构造函数的方式，每个函数可以接收多对和[ICorProfilerCallback：： JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)调用。 例如，运行时开始 JIT 编译方法 A，但类 B 的类构造函数需要运行。 因此，运行时 JIT 编译类 B 的构造函数并运行它。 当构造函数正在运行时，它会调用方法 A，这将导致再次 JIT 编译方法 A。 在此方案中，方法 A 的第一次 JIT 编译将暂停。 但是，将使用 JIT 编译事件来报告 JIT 编译方法 A 的两种尝试。 如果探查器将使用 [ICorProfilerInfo：： SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md) 方法替换方法 A 的 Microsoft 中间语言 (MSIL) 代码，它必须为这两个 `JITCompilationStarted` 事件都使用同一 msil 块。  
  
 当两个线程同时进行回调时，探查器必须支持 JIT 回调的序列。 例如，线程 A 调用 `JITCompilationStarted` 。 但是，在线程 A 调用之前 `JITCompilationFinished` ，线程 B 使用线程 A 的回调中的函数 ID 调用 [ICorProfilerCallback：： ExceptionSearchFunctionEnter](icorprofilercallback-exceptionsearchfunctionenter-method.md) `JITCompilationStarted` 。 由于探查器尚未收到对的调用，因此该函数 ID 可能会似乎无效 `JITCompilationFinished` 。 但是，在这种情况下，函数 ID 有效。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [JITCompilationFinished 方法](icorprofilercallback-jitcompilationfinished-method.md)
