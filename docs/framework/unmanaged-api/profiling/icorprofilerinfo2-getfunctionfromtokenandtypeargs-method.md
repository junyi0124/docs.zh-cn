---
title: ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetFunctionFromTokenAndTypeArgs
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs
helpviewer_keywords:
- ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
- GetFunctionFromTokenAndTypeArgs method [.NET Framework profiling]
ms.assetid: ce8f6aa6-4ebf-4a86-b429-4bbc8af41a8f
topic_type:
- apiref
ms.openlocfilehash: 17a6220598010c0bee9c3f0485860aa0b2dc5f3a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727100"
---
# <a name="icorprofilerinfo2getfunctionfromtokenandtypeargs-method"></a>ICorProfilerInfo2::GetFunctionFromTokenAndTypeArgs 方法

`FunctionID`使用指定的元数据标记（包含类）和 `ClassID` 任何类型参数的值获取函数的。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFunctionFromTokenAndTypeArgs(  
    [in] ModuleID moduleID,  
    [in] mdMethodDef funcDef,  
    [in] ClassID classId,  
    [in] ULONG32 cTypeArgs,  
    [in, size_is(cTypeArgs)] ClassID typeArgs[],  
    [out] FunctionID* pFunctionID);  
```  
  
## <a name="parameters"></a>参数  

 `moduleID`  
 中函数所在模块的 ID。  
  
 `funcDef`  
 中 `mdMethodDef` 引用函数的元数据标记。  
  
 `classId`  
 中函数的包含类的 ID。  
  
 `cTypeArgs`  
 中给定函数的类型参数的数目。 对于非泛型函数，此值必须为零。  
  
 `typeArgs`  
 中值的数组 `ClassID` ，其中每个值都是函数的参数。 如果设置为零，则的值 `typeArgs` 可以为 NULL `cTypeArgs` 。  
  
 `pFunctionID`  
 弄指向指定函数的的指针 `FunctionID` 。  
  
## <a name="remarks"></a>注解  

 `GetFunctionFromTokenAndTypeArgs`使用 `mdMethodRef` 元数据而不是 `mdMethodDef` 元数据标记调用方法可能会产生不可预知的结果。 传递时，调用方应将解析 `mdMethodRef` 为 `mdMethodDef` 。  
  
 如果尚未加载该函数，则调用 `GetFunctionFromTokenAndTypeArgs` 将导致加载，这在许多上下文中是一个危险操作。 例如，在模块或类型加载过程中调用此方法可能会导致无限循环，因为运行时尝试循环加载某些功能。  
  
 通常， `GetFunctionFromTokenAndTypeArgs` 不建议使用。 如果探查器对特定函数的事件感兴趣，则它们应存储 `ModuleID` `mdMethodDef` 该函数的和，并使用 [ICorProfilerInfo2：： GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) 来检查给定的是否 `FunctionID` 是所需函数的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
