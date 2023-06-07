---
title: ICorProfilerCallback3 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback3
helpviewer_keywords:
- ICorProfilerCallback3 interface [.NET Framework profiling]
ms.assetid: be83af41-3dec-4c77-8529-9dd6b8042af6
topic_type:
- apiref
ms.openlocfilehash: fd482bfe8e95a53cafd1530c88f09df146a1b150
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729429"
---
# <a name="icorprofilercallback3-interface"></a>ICorProfilerCallback3 接口

提供公共语言运行时 (CLR) 用来向探查器传达附加和分离状态信息的回调方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[InitializeForAttach 方法](icorprofilercallback3-initializeforattach-method.md)|由 CLR 调用，以使探查器能够在附加操作后初始化其状态。|  
|[ProfilerAttachComplete 方法](icorprofilercallback3-profilerattachcomplete-method.md)|由 CLR 调用以指示探查器现在可以调用追赶方法。|  
|[ProfilerDetachSucceeded 方法](icorprofilercallback3-profilerdetachsucceeded-method.md)|通知探查器公共语言运行时 (CLR) 将要卸载探查器 DLL。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
- [ICorProfilerCallback4 接口](icorprofilercallback4-interface.md)
