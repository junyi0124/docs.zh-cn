---
title: ICorProfilerInfo::ForceGC 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.ForceGC
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::ForceGC
helpviewer_keywords:
- ICorProfilerInfo::ForceGC method [.NET Framework profiling]
- ForceGC method [.NET Framework profiling]
ms.assetid: 0da1ef80-d242-4636-87d0-43e0470b342a
topic_type:
- apiref
ms.openlocfilehash: 75f2db5e5489f02dcae3db0c3e6af19c922e0745
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669188"
---
# <a name="icorprofilerinfoforcegc-method"></a>ICorProfilerInfo::ForceGC 方法

强制在公共语言运行时 (CLR) 中发生垃圾回收。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ForceGC();  
```  
  
## <a name="remarks"></a>备注  

 `ForceGC`只能从从未运行托管代码并且在其堆栈上没有任何探查器回调的线程调用方法。 最方便的实现是在探查器中创建一个单独的线程，该线程在 `ForceGC` 收到信号时调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
