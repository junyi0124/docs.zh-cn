---
title: ICorProfilerInfo3::RequestProfilerDetach 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo3.RequestProfilerDetach Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo3::RequestProfilerDetach
helpviewer_keywords:
- RequestProfilerDetach method [.NET Framework profiling]
- ICorProfilerInfo3::RequestProfilerDetach method [.NET Framework profiling]
ms.assetid: ea102e62-0454-4477-bcf3-126773acd184
topic_type:
- apiref
ms.openlocfilehash: 2ea39c94a5a0f3d24d4123d6405115ac75105e26
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721577"
---
# <a name="icorprofilerinfo3requestprofilerdetach-method"></a>ICorProfilerInfo3::RequestProfilerDetach 方法

指示运行时分离探查器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RequestProfilerDetach(  
   [in] DWORD    dwExpectedCompletionMilliseconds);  
```  
  
## <a name="parameters"></a>参数  

 `dwExpectedCompletionMilliseconds`  
 [in] 公共语言运行时 (CLR) 在检查卸载探查器是否安全之前应等待的事件长度（以毫秒计）。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|分离请求有效，且分离过程现在另一线程上继续执行。 完全分离后，将发出 `ProfilerDetachSucceeded` 事件。|  
|E_CORPROF_E_CALLBACK3_REQUIRED|探查器未能通过[ICorProfilerCallback3](icorprofilercallback3-interface.md)接口的[IUnknown：： QueryInterface](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q))尝试，该接口必须实现该接口才能支持分离操作。 尚未尝试分离。|  
|CORPROF_E_IMMUTABLE_FLAGS_SET|无法进行分离，因为探查器在启动时设置了不可变标志。 尚未尝试分离；探查器仍处于完全附加状态。|  
|CORPROF_E_IRREVERSIBLE_INSTRUMENTATION_PRESENT|分离是不可能的，因为探查器使用检测到 Microsoft 中间语言 (MSIL) 代码或插入的 `enter` / `leave` 挂钩。 尚未尝试分离；探查器仍处于完全附加状态。<br /><br /> **注意** 已检测 MSIL 是代码，由探查器使用 [SetILFunctionBody](icorprofilerinfo-setilfunctionbody-method.md) 方法提供。|  
|CORPROF_E_RUNTIME_UNINITIALIZED|托管应用程序中的运行时尚未初始化。  (即，未完全加载运行时。在探查器回调的 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md) 方法中请求分离时，可能会返回此错误代码 ) 。|  
|CORPROF_E_UNSUPPORTED_CALL_SEQUENCE|在不支持时调用了 `RequestProfilerDetach`。 如果在托管线程上调用方法，而不是从 [ICorProfilerCallback](icorprofilercallback-interface.md) [方法中](icorprofilercallback-interface.md) 调用，或者不能容忍垃圾回收，则会发生这种情况。 有关详细信息，请参阅 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT](corprof-e-unsupported-call-sequence-hresult.md)。|  
  
## <a name="remarks"></a>注解  

 在分离过程中，分离线程（专为分离探查器创建的线程）有时会检查是否所有线程均已退出探查器的代码。 探查器应通过 `dwExpectedCompletionMilliseconds` 参数估计此操作的耗时。 最佳使用值是探查器在任何给定 `ICorProfilerCallback*` 方法内通常花费的时间量；此值不应小于探查器预计花费时间量的一半。  
  
 分离线程使用 `dwExpectedCompletionMilliseconds` 决定在检查探查器回调代码是否已从所有堆栈中弹出之前需要休眠多长时间。 尽管以下算法的详细信息在 CLR 的未来版本中可能有所更改，但它展示了在确定何时可安全卸载探查器时可使用 `dwExpectedCompletionMilliseconds` 的一种方法。 分离线程先休眠 `dwExpectedCompletionMilliseconds` 毫秒。 如果在唤醒后，CLR 发现探查器回调代码仍存在，分离线程将再次休眠，这一次是两次 `dwExpectedCompletionMilliseconds` 。 如果从第二次休眠状态唤醒后，分离线程仍发现存在探查器回调代码，则将休眠 10 分钟再进行检查。 分离线程每隔 10 分钟继续进行重新检查。  
  
 如果探查器将 `dwExpectedCompletionMilliseconds` 指定为 0（零），CLR 使用默认值 5000，这意味着探查器在 5 秒钟后执行检查，10 秒后再次检查，然后每隔 10 分钟进行重新检查。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
