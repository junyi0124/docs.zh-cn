---
title: ICorProfilerInfo::BeginInprocDebugging 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.BeginInprocDebugging
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::BeginInprocDebugging
helpviewer_keywords:
- BeginInprocDebugging method [.NET Framework profiling]
- ICorProfilerInfo::BeginInprocDebugging method [.NET Framework profiling]
ms.assetid: c5c82c69-99f8-4447-aee0-42cca0a5eb5c
topic_type:
- apiref
ms.openlocfilehash: 3f56c3faa10eb05896936a37b0094797b0b6e2b9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669167"
---
# <a name="icorprofilerinfobegininprocdebugging-method"></a>ICorProfilerInfo::BeginInprocDebugging 方法

初始化进程内调试支持。 此方法在 .NET Framework 版本2.0 中已过时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT BeginInprocDebugging(  
    [in]  BOOL   fThisThreadOnly,  
    [out] DWORD *pdwProfilerContext);  
```  
  
## <a name="parameters"></a>参数  

 `fThisThreadOnly`  
 中将此值设置为 `true` 以仅初始化当前线程的调试支持; 将其设置为 `false` 以初始化对所有线程的调试支持。  
  
 `pdwProfilerContext`  
 弄指向用于标识调试会话的返回值的指针。  
  
## <a name="remarks"></a>注解  

 CLR 调试服务支持 .NET Framework 版本1.0 和1.1 中的有限进程内调试。 进程内调试使探查器能够使用调试 API 的检查部分。 不过，由于客户反馈，已从版本2.0 中的 .NET Framework 中删除进程内调试，并将其替换为一组功能，这些功能与分析 API 行更详细。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1。0  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
