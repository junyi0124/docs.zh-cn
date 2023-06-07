---
title: ICorProfilerInfo2::GetFunctionInfo2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetFunctionInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetFunctionInfo2
helpviewer_keywords:
- GetFunctionInfo2 method [.NET Framework profiling]
- ICorProfilerInfo2::GetFunctionInfo2 method [.NET Framework profiling]
ms.assetid: 0aa60f24-8bbd-4c83-83c5-86ad191b1d82
topic_type:
- apiref
ms.openlocfilehash: e44b8afe22fdb10077048dc7bc2ccb1f605edd75
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727089"
---
# <a name="icorprofilerinfo2getfunctioninfo2-method"></a>ICorProfilerInfo2::GetFunctionInfo2 方法

获取每个类型参数或某个函数（如果存在）的父类、元数据标记和 `ClassID`。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFunctionInfo2(  
    [in]  FunctionID funcId,  
    [in]  COR_PRF_FRAME_INFO frameInfo,  
    [out] ClassID *pClassId,  
    [out] ModuleID *pModuleId,  
    [out] mdToken *pToken,  
    [in]  ULONG32 cTypeArgs,  
    [out] ULONG32 *pcTypeArgs,  
    [out] ClassID typeArgs[]);  
```  
  
## <a name="parameters"></a>参数  

 `funcId`  
 [in] 要获取其父类和其他信息的函数的 ID。  
  
 `frameInfo`  
 [in] 一个 `COR_PRF_FRAME_INFO` 值，该值指向有关堆栈帧的信息。  
  
 `pClassId`  
 [out] 一个指向函数的父类的指针。  
  
 `pModuleId`  
 [out] 一个指向在其中定义函数父类的模块的指针。  
  
 `pToken`  
 [out] 指向函数的元数据标记的指针。  
  
 `cTypeArgs`  
 [in] `typeArgs` 数组的大小。  
  
 `pcTypeArgs`  
 [out] 一个指向 `ClassID` 值的总数的指针。  
  
 `typeArgs`  
 [out] 一个由 `ClassID` 值构成的数组，其中的每个值都是函数的类型参数的 ID。 方法返回时，`typeArgs` 将包含部分或全部 `ClassID` 值。  
  
## <a name="remarks"></a>注解  

 探查器代码可调用 [ICorProfilerInfo：： GetModuleMetaData](icorprofilerinfo-getmodulemetadata-method.md) 以获取给定模块的 [元数据](../metadata/index.md) 接口。 然后，返回到 `pToken` 所引用位置的元数据标记便可用于访问该函数的元数据。  
  
 通过 `pClassId` 和 `typeArgs` 参数返回的类 ID 和类型参数取决于传入 `frameInfo` 参数的值，如下表中所示。  
  
|`frameInfo` 参数的值|结果|  
|----------------------------------------|------------|  
|从 `FunctionEnter2` 回调中获得的 `COR_PRF_FRAME_INFO` 值|在 `pClassId` 所引用位置中返回的 `ClassID` 以及在 `typeArgs` 数组中返回的所有类型参数都将是准确的。|  
|从 `FunctionEnter2` 回调以外的源中获得的 `COR_PRF_FRAME_INFO`|无法确定准确的 `ClassID` 和类型参数。 也就是说，`ClassID` 可能为 NULL，并且某些类型参数可能作为 <xref:System.Object> 返回。|  
|零个|无法确定准确的 `ClassID` 和类型参数。 也就是说，`ClassID` 可能为 NULL，并且某些类型参数可能作为 <xref:System.Object> 返回。|  
  
 `GetFunctionInfo2` 返回后，必须验证 `typeArgs` 缓冲区是否足够大，可包含所有 `ClassID` 值。 为此，请比较 `pcTypeArgs` 指向的值和 `cTypeArgs` 参数的值。 如果 `pcTypeArgs` 指向大于`cTypeArgs` 除以 `ClassID` 值大小的值，请分配更大的 `pcTypeArgs` 缓冲区，使用新的、更大的大小更新 `cTypeArgs`，然后再次调用 `GetFunctionInfo2`。  
  
 或者，可以先用长度为零的 `pcTypeArgs` 缓冲区调用 `GetFunctionInfo2` 以获取正确的缓冲区大小。 然后，可将缓冲区大小设置为 `pcTypeArgs` 中返回的值除以 `ClassID` 值的大小，然后再次调用 `GetFunctionInfo2`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
