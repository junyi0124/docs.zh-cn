---
title: ICorProfilerCallback8 接口
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 22a133d02bb69026190428905379323362943d40
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732380"
---
# <a name="icorprofilercallback8-interface"></a>ICorProfilerCallback8 接口

[.NET Framework 4.7 及更高版本中支持]  

 提供回调方法的 [ICorProfilerCallback7](icorprofilercallback7-interface.md) 的子类，公共语言运行时使用该方法通知探查器动态方法的 JIT 编译已开始和完成。
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[DynamicMethodJITCompilationStarted 方法](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)|通知探查器已经开始对动态方法进行 JIT 编译。|  
|[DynamicMethodJITCompilationFinished 方法](icorprofilercallback8-dynamicmethodjitcompilationfinished-method.md)|通知探查器已完成动态方法的 JIT 编译。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback9 接口](icorprofilercallback9-interface.md)
