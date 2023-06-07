---
title: ICorProfilerCallback5 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback5
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback5
helpviewer_keywords:
- ICorProfilerCallback5 interface [.NET Framework profiling]
ms.assetid: 61d2e9ef-5f1f-4771-8847-239616e35d84
topic_type:
- apiref
ms.openlocfilehash: 8e94aebc489fff1c81593e54b17c471e7228d810
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689285"
---
# <a name="icorprofilercallback5-interface"></a>ICorProfilerCallback5 接口

当与 [ICorProfilerCallback：： RootReferences](icorprofilercallback-rootreferences-method.md) 或 [ICorProfilerCallback2：： RootReferences2](icorprofilercallback2-rootreferences2-method.md) 方法一起使用来帮助探查器识别完整的实时对象闭包和 [ICorProfilerCallback：： ObjectReferences](icorprofilercallback-objectreferences-method.md) 和 [ConditionalWeakTableElementReferences](icorprofilercallback5-conditionalweaktableelementreferences-method.md) 方法。  
  
 `ICorProfilerCallback5` 必须由托管的内存探查器实现，才能订阅与依赖句柄相关的通知。  
  
## <a name="remarks"></a>注解  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ConditionalWeakTableElementReferences 方法](icorprofilercallback5-conditionalweaktableelementreferences-method.md)|标识这些根通过直接成员字段引用和 `ConditionalWeakTable` 依赖关系引用的对象的传递闭包。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback2 接口](icorprofilercallback2-interface.md)
