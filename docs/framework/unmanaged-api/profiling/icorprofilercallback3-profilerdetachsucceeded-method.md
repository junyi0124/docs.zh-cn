---
title: ICorProfilerCallback3::ProfilerDetachSucceeded 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3.ProfilerDetachSucceeded Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3::ProfilerDetachSucceeded
helpviewer_keywords:
- ProfilerDetachSucceeded method [.NET Framework profiling]
- ICorProfilerCallback3::ProfilerDetachSucceeded method [.NET Framework profiling]
ms.assetid: 05164966-16ce-4cc9-a530-43a640c00711
topic_type:
- apiref
ms.openlocfilehash: b9b284de102dc75a637803ca5be0f2769da452ec
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730313"
---
# <a name="icorprofilercallback3profilerdetachsucceeded-method"></a>ICorProfilerCallback3::ProfilerDetachSucceeded 方法

通知探查器公共语言运行时 (CLR) 将要卸载探查器 DLL。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ProfilerDetachSucceeded();  
```  
  
## <a name="return-value"></a>返回值  

 将忽略来自此回调的返回值。  
  
## <a name="remarks"></a>注解  

 在所有线程均退出探查器的代码之后，发出`ProfilerDetachSucceeded` 回调。 当调用此方法时，探查器应执行任何不适合用于其析构函数的的最后执行的任务，例如通知其 UI 或日志记录组件。 但是，探查器不能在此 (回调期间（如 [ICorProfilerInfo](icorprofilerinfo-interface.md) 或接口) ）调用 CLR 提供的接口上的函数 `IMetaData*` 。  
  
 CLR 在 Windows 应用程序事件日志中创建条目，用于表示分离操作成功。  
  
 探查器从此回调返回后，CLR 将释放探查器对象并卸载探查器 DLL。 因此，探查器不可执行任何会导致探查器 DLL 从此回调返回后其内部进行执行的操作。 例如，它不能创建线程或注册计时器回调。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据接口](../metadata/metadata-interfaces.md)
- [ICorProfilerInfo3 接口](icorprofilerinfo3-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
