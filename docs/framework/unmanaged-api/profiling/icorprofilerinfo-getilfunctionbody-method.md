---
title: ICorProfilerInfo::GetILFunctionBody 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetILFunctionBody
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetILFunctionBody
helpviewer_keywords:
- GetILFunctionBody method [.NET Framework profiling]
- ICorProfilerInfo::GetILFunctionBody method [.NET Framework profiling]
ms.assetid: e29b46bc-5fdc-4894-b0c2-619df4b65ded
topic_type:
- apiref
ms.openlocfilehash: 337c4fd091ebf7c39f7eee2358ca4f4df239cce3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687523"
---
# <a name="icorprofilerinfogetilfunctionbody-method"></a>ICorProfilerInfo::GetILFunctionBody 方法

获取一个指针，该指针指向 Microsoft 中间语言 (MSIL) 代码的方法体，从其标头开始。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetILFunctionBody(  
    [in]  ModuleID    moduleId,  
    [in]  mdMethodDef methodId,  
    [out] LPCBYTE     *ppMethodHeader,  
    [out] ULONG       *pcbMethodSize);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 中函数所在模块的 ID。  
  
 `methodId`  
 中方法的元数据标记。  
  
 `ppMethodHeader`  
 弄指向方法的标头的指针。  
  
 `pcbMethodSize`  
 弄一个整数，指定方法的大小。  
  
## <a name="remarks"></a>注解  

 方法的作用域由其所在的模块确定。 由于 `GetILFunctionBody` 方法旨在在公共语言运行时 (CLR) 加载 MSIL 代码之前提供对这些代码的访问权限，因此它使用方法的元数据标记来查找所需的实例。  
  
 `GetILFunctionBody` 如果 `methodId` 指向不包含任何 MSIL 代码的方法 (（如抽象方法）或平台调用 (PInvoke) 方法) ，则可以返回 CORPROF_E_FUNCTION_NOT_IL HRESULT。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
