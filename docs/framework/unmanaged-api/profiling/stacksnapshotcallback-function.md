---
title: StackSnapshotCallback 函数
ms.date: 03/30/2017
api_name:
- StackSnapshotCallback
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- StackSnapshotCallback
helpviewer_keywords:
- StackSnapshotCallback function [.NET Framework profiling]
ms.assetid: d0f235b2-91fe-4f82-b7d5-e5c64186eea8
topic_type:
- apiref
ms.openlocfilehash: 2d6ca18ce48f69d8c94b465efac2b9fe0e10f070
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685300"
---
# <a name="stacksnapshotcallback-function"></a>StackSnapshotCallback 函数

在堆栈遍历期间，为探查器提供有关每个托管帧和每个托管帧的每个运行的信息，由 [ICorProfilerInfo2：:D ostacksnapshot](icorprofilerinfo2-dostacksnapshot-method.md) 方法启动。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT __stdcall StackSnapshotCallback (  
    [in] FunctionID funcId,  
    [in] UINT_PTR ip,  
    [in] COR_PRF_FRAME_INFO frameInfo,  
    [in] ULONG32 contextSize,  
    [in] BYTE context[],  
    [in] void *clientData  
);  
```  
  
## <a name="parameters"></a>参数  

 `funcId`  
 中如果此值为零，则此回调适用于非托管帧的运行;否则，它是托管函数的标识符，此回调适用于托管帧。  
  
 `ip`  
 中帧中的本机代码指令指针的值。  
  
 `frameInfo`  
 中一个 `COR_PRF_FRAME_INFO` 值，该值引用有关堆栈帧的信息。 此值仅在此回调期间有效。  
  
 `contextSize`  
 中 `CONTEXT` 由参数引用的结构的大小 `context` 。  
  
 `context`  
 中指向 Win32 `CONTEXT` 结构的指针，该结构表示此帧的 CPU 的状态。  
  
 `context`仅当传入 COR_PRF_SNAPSHOT_CONTEXT 标志时，此参数才有效 `ICorProfilerInfo2::DoStackSnapshot` 。  
  
 `clientData`  
 中指向客户端数据的指针，该数据是通过从直接传递的 `ICorProfilerInfo2::DoStackSnapshot` 。  
  
## <a name="remarks"></a>注解  

 `StackSnapshotCallback`函数由探查器编写器实现。 必须限制中完成的工作的复杂性 `StackSnapshotCallback` 。 例如， `ICorProfilerInfo2::DoStackSnapshot` 以异步方式使用时，目标线程可能会持有锁。 如果中 `StackSnapshotCallback` 的代码需要相同的锁，则可能会不幸死锁。  
  
 `ICorProfilerInfo2::DoStackSnapshot`方法对 `StackSnapshotCallback` 每个托管帧调用一次函数，或在每次运行非托管帧时调用函数。 如果 `StackSnapshotCallback` 调用来运行非托管帧，则探查器可能会使用参数) 引用 (注册上下文 `context` 来执行其自己的非托管堆栈遍历。 在这种情况下，Win32 `CONTEXT` 结构表示在非托管帧运行内最近推送的帧的 CPU 状态。 尽管 Win32 `CONTEXT` 结构包含所有寄存器的值，但你只应依赖于堆栈指针寄存器、帧指针寄存器、指令指针寄存器和非易失性 (的值，这些值将保留) 整数寄存器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corprof.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [DoStackSnapshot 方法](icorprofilerinfo2-dostacksnapshot-method.md)
- [分析全局静态函数](profiling-global-static-functions.md)
