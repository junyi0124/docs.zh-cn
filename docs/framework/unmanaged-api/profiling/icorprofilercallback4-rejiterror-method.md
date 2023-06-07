---
title: ICorProfilerCallback4::ReJITError 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback4.ReJITError
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback4::ReJITError
helpviewer_keywords:
- ReJITError method, ICorProfilerCallback4 interface [.NET Framework profiling]
- ICorProfilerCallback4::ReJITError method [.NET Framework profiling]
ms.assetid: d7888aa9-dfaa-420f-9f99-e06ab35ca482
topic_type:
- apiref
ms.openlocfilehash: 46312aaf530e69f0e6a90e35515f1373d01b4340
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730235"
---
# <a name="icorprofilercallback4rejiterror-method"></a>ICorProfilerCallback4::ReJITError 方法

通知探查器实时 (JIT) 编译器在重新编译过程中遇到错误。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReJITError(  
    [in] ModuleID    moduleId,  
    [in] mdMethodDef methodId,  
    [in] FunctionID  functionId,  
    [in] HRESULT     hrStatus);  
```  
  
## <a name="parameters"></a>参数  

 `moduleID`  
 中 `ModuleID` 尝试重新编译失败的。  
  
 `methodId`  
 中 `MethodDef` 尝试重新编译失败的方法的。  
  
 `functionId`  
 中正在重新编译或标记为要重新编译的函数实例。 `NULL`如果故障发生在每个方法的基础上而不是每个实例化的基础上，此值可能为 (例如，如果探查器为要重新编译的方法指定了无效的元数据标记) 。  
  
 `hrStatus`  
 中指示失败性质的 HRESULT。 有关值的列表，请参阅状态 HRESULT 部分。  
  
## <a name="return-value"></a>返回值  

 将忽略此回调的返回值。  
  
## <a name="status-hresults"></a>状态 HRESULTS  
  
|状态数组 HRESULT|说明|  
|--------------------------|-----------------|  
|E_INVALIDARG|`moduleID`或 `methodDef` 标记为 `NULL` 。|  
|CORPROF_E_DATAINCOMPLETE|该模块尚未完全加载，或正在被卸载。|  
|CORPROF_E_MODULE_IS_DYNAMIC|指定的模块是动态生成的 (例如 `Reflection.Emit`) ，因此不受此方法支持。|  
|CORPROF_E_FUNCTION_IS_COLLECTIBLE|方法被实例化为可回收的程序集，因此无法重新编译。 请注意，在非反射上下文中定义的类型和函数 (例如， `List<MyCollectibleStruct>`) 可以实例化为可回收的程序集。|  
|E_OUTOFMEMORY|尝试将指定的方法标记为 JIT 重新编译时，CLR 用尽了内存。|  
|其他|操作系统返回了 CLR 控件范围之外的失败。 例如，如果系统调用更改内存页的访问保护失败，则会显示操作系统错误。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ICorProfilerCallback4 接口](icorprofilercallback4-interface.md)
