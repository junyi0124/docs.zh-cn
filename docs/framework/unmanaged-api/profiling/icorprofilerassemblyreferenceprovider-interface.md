---
title: ICorProfilerAssemblyReferenceProvider 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerAssemblyReferenceProvider
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 17205116-66e1-4acc-8f01-532fb3867028
topic_type:
- apiref
ms.openlocfilehash: 2cee012be2665f5a7212600ec11e401b18160d8b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95691391"
---
# <a name="icorprofilerassemblyreferenceprovider-interface"></a>ICorProfilerAssemblyReferenceProvider 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 使探查器能够向公共语言运行 (时通知) 探查器将在 [ICorProfilerCallback：： ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调中添加的程序集引用的 CLR。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AddAssemblyReference 方法](icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)|向 CLR 通知探查器计划在 [ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调中添加的程序集引用。|  
  
## <a name="remarks"></a>注解  

 CLR 将探查器的 `ICorProfilerAssemblyReferenceProvider` 接口对象传递给 [ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) 回调。 这使探查器能够通知程序集引用的 CLR，探查器计划稍后将其添加到 [ICorProfilerCallback：： ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md)中。 回调中。 这提高了 CLR 的程序集引用闭包审核器，及其用于确定是否可以共享程序集的算法的准确性。  
  
 此接口只能在将此接口对象传递给探查器的 [ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) 回调中使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
