---
title: ICorProfilerInfo::GetInprocInspectionInterface 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetInprocInspectionInterface
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetInprocInspectionInterface
helpviewer_keywords:
- GetInprocInspectionInterface method [.NET Framework profiling]
- ICorProfilerInfo::GetInprocInspectionInterface method [.NET Framework profiling]
ms.assetid: 22a92d1d-8849-4af6-8304-ecc53dd1d289
topic_type:
- apiref
ms.openlocfilehash: cc8bdfb1e46e5304227a40f869856f07e1f90bed
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707485"
---
# <a name="icorprofilerinfogetinprocinspectioninterface-method"></a>ICorProfilerInfo::GetInprocInspectionInterface 方法

获取一个对象，该对象可查询 "ICorDebugProcess" 接口。 此方法在 .NET Framework 版本2.0 中已过时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetInprocInspectionInterface(  
    [out] IUnknown **ppicd);  
```  
  
## <a name="parameters"></a>参数  

 `ppicd`  
 可在接口上查询的[out](/cpp/atl/iunknown)对象 `ICorDebugProcess` 。  
  
## <a name="remarks"></a>注解  

 公共语言运行时 (CLR) 调试 API 在 .NET Framework 版本1.0 中支持有限的进程内调试。 进程内调试使探查器能够使用调试 API 的检查部分。 由于客户反馈，已从版本2.0 中的 .NET Framework 中删除进程内调试，并将其替换为一组功能，这些功能与分析 API 是一种更多的功能。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1。0  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
