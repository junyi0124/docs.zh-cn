---
title: ICorProfilerCallback6 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback6
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.assetid: edc420b7-96d1-4dec-ace0-36d907f644bc
topic_type:
- apiref
ms.openlocfilehash: 2176b624a427994b9d2af4b5eba31a64c9288a0e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725464"
---
# <a name="icorprofilercallback6-interface"></a>ICorProfilerCallback6 接口

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 提供回调方法的 [ICorProfilerCallback5](icorprofilercallback5-interface.md) 的子类，公共语言运行时使用该方法通知探查器正在加载程序集。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetAssemblyReferences 方法](icorprofilercallback6-getassemblyreferences-method.md)|当公共语言运行时执行程序集引用闭包审核时，通知探查器程序集正处于非常早期的加载阶段。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
