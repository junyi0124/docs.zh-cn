---
title: FunctionEnter2 函数
ms.date: 03/30/2017
api_name:
- FunctionEnter2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionEnter2
helpviewer_keywords:
- FunctionEnter2 function [.NET Framework profiling]
ms.assetid: ce7a21f9-0ca3-4b92-bc4b-bb803cae3f51
topic_type:
- apiref
ms.openlocfilehash: e8466970a1c137276e842b37f0305fdfd9169be6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717217"
---
# <a name="functionenter2-function"></a>FunctionEnter2 函数

通知探查器控制正在传递到函数，并提供有关堆栈帧和函数参数的信息。 此函数取代了 [FunctionEnter](functionenter-function.md) 函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
void __stdcall FunctionEnter2 (  
    [in]  FunctionID                       funcId,
    [in]  UINT_PTR                         clientData,
    [in]  COR_PRF_FRAME_INFO               func,
    [in]  COR_PRF_FUNCTION_ARGUMENT_INFO  *argumentInfo  
);  
```  
  
## <a name="parameters"></a>参数

- `funcId`

  \[in] 控件传递到的函数的标识符。

- `clientData`

  \[in] 重新映射的函数标识符，该标识符以前使用 [FunctionIDMapper](functionidmapper-function.md) 函数指定了探查器。
  
- `func`

  \[in] 一个 `COR_PRF_FRAME_INFO` 值，该值指向有关堆栈帧的信息。
  
  探查器应将此视为不透明的句柄，该句柄可传递回 [ICorProfilerInfo2：： GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) 方法中的执行引擎。  
  
- `argumentInfo`

  \[in] 一个指向 [COR_PRF_FUNCTION_ARGUMENT_INFO](cor-prf-function-argument-info-structure.md) 结构的指针，该结构指定函数参数在内存中的位置。

  为了访问参数信息， `COR_PRF_ENABLE_FUNCTION_ARGS` 必须设置标志。 探查器可以使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法来设置事件标志。

## <a name="remarks"></a>注解  

 `func` `argumentInfo` 当函数返回后，和参数的值无效， `FunctionEnter2` 因为值可能会更改或被销毁。  
  
 `FunctionEnter2`函数是回调; 必须实现它。 实现必须使用 `__declspec` `naked`) 存储类特性的 (。  
  
 在调用此函数之前，执行引擎不会保存任何注册。  
  
- 进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。  
  
- 退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。  
  
 的实现 `FunctionEnter2` 不应被阻止，因为它将延迟垃圾回收。 实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。 如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionEnter2` 返回。  
  
 此外，该 `FunctionEnter2` 函数不得调入托管代码或以任何方式导致托管的内存分配。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corprof.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [FunctionLeave2 函数](functionleave2-function.md)
- [FunctionTailcall2 函数](functiontailcall2-function.md)
- [SetEnterLeaveFunctionHooks2 方法](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [分析全局静态函数](profiling-global-static-functions.md)
