---
title: ICorProfilerInfo::SetILFunctionBody 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetILFunctionBody
helpviewer_keywords:
- ICorProfilerInfo::SetILFunctionBody method [.NET Framework profiling]
- SetILFunctionBody method [.NET Framework profiling]
ms.assetid: b159c712-00f4-4fc7-a990-40bf9f642e8f
topic_type:
- apiref
ms.openlocfilehash: 376b9fc637993f00722c48db7f51650e0a22d931
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720914"
---
# <a name="icorprofilerinfosetilfunctionbody-method"></a>ICorProfilerInfo::SetILFunctionBody 方法

替换指定模块中指定函数的主体。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetILFunctionBody(  
    [in] ModuleID    moduleId,  
    [in] mdMethodDef methodid,  
    [in] LPCBYTE     pbNewILMethodHeader);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 中函数所在模块的 ID。  
  
 `methodid`  
 中要替换其正文的函数的标记。  
  
 `pbNewILMethodHeader`  
 中函数的新标头。  
  
## <a name="remarks"></a>注解  

 `SetILFunctionBody`方法替换元数据中函数的相对虚拟地址，以使其指向新的函数体，并根据需要调整任何内部数据结构。  
  
 `SetILFunctionBody`仅可对从未由实时 (JIT) 编译器编译的函数调用方法。。  
  
 使用 [ICorProfilerInfo：： GetILFunctionBodyAllocator](icorprofilerinfo-getilfunctionbodyallocator-method.md) 方法为新方法分配空间，以确保缓冲区兼容。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
