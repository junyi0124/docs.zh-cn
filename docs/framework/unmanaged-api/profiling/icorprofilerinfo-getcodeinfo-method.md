---
title: ICorProfilerInfo::GetCodeInfo 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetCodeInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetCodeInfo
helpviewer_keywords:
- GetCodeInfo method [.NET Framework profiling]
- ICorProfilerInfo::GetCodeInfo method [.NET Framework profiling]
ms.assetid: 90140b0f-a926-4a7e-b6fa-23e05f703cce
topic_type:
- apiref
ms.openlocfilehash: da3b0acefa68be3506567ad4742784943f4ec5ab
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722513"
---
# <a name="icorprofilerinfogetcodeinfo-method"></a>ICorProfilerInfo::GetCodeInfo 方法

获取与指定函数 ID 关联的本机代码的范围。  
  
 此方法已过时。 改为使用 [ICorProfilerInfo2：： GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCodeInfo(  
    [in]  FunctionID functionId,  
    [out] LPCBYTE    *pStart,  
    [out] ULONG      *pcSize);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 [in] 与本机代码关联的函数的 ID。  
  
 `pStart`  
 [out] 指向组成该函数本机代码的字节数组的指针。  
  
 `pcSize`  
 [out] 指向以字节为单位指定本机代码大小的整数的指针。  
  
## <a name="remarks"></a>注解  

 为了优化性能，.NET Framework 2.0 版中的运行时将函数的预编译本机代码拆分为多个部分。 因此，`GetCodeInfo` 方法在.NET Framework 2.0 中已过时，因为它无法处理函数的本机代码的范围。 探查器应切换为使用更常规的 `ICorProfilerInfo2::GetCodeInfo2` 方法。  
  
 此函数使用调用方分配的缓冲区。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1。0  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
