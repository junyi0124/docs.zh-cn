---
title: ICorProfilerInfo::EndInprocDebugging 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.EndInprocDebugging
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::EndInprocDebugging
helpviewer_keywords:
- ICorProfilerInfo::EndInprocDebugging method [.NET Framework profiling]
- EndInprocDebugging method [.NET Framework profiling]
ms.assetid: 35bc1188-9767-4141-8038-60ea015b99ac
topic_type:
- apiref
ms.openlocfilehash: e929c74ba0f1f33ddbb28476b3c9e0a512567ac6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669050"
---
# <a name="icorprofilerinfoendinprocdebugging-method"></a>ICorProfilerInfo::EndInprocDebugging 方法

关闭进程内调试会话。 此方法在 .NET Framework 版本2.0 中已过时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EndInprocDebugging(  
    [in]  DWORD dwProfilerContext);  
```  
  
## <a name="parameters"></a>参数  

 `dwProfilerContext`  
 中一个值，该值标识调试会话。 此值必须与在 [ICorProfilerInfo：： BeginInprocDebugging](icorprofilerinfo-begininprocdebugging-method.md) 方法中接收的值相同。  
  
## <a name="remarks"></a>注解  

 必须在同一回调方法中调用 [ICorProfilerInfo：： BeginInprocDebugging](icorprofilerinfo-begininprocdebugging-method.md) 和 `EndInprocDebugging` 。  
  
 CLR 调试服务支持 .NET Framework 版本1.0 和1.1 中的有限进程内调试。 进程内调试使探查器能够使用调试 API 的检查部分。 不过，由于客户反馈，已从版本2.0 中的 .NET Framework 中删除进程内调试，并将其替换为一组功能，这些功能与分析 API 行更详细。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1。0  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
