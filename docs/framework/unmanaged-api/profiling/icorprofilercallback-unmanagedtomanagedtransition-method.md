---
title: ICorProfilerCallback::UnmanagedToManagedTransition 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.UnmanagedToManagedTransition
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition
helpviewer_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition method [.NET Framework profiling]
- UnmanagedToManagedTransition method [.NET Framework profiling]
ms.assetid: ade2cc01-9b81-4e09-a5f9-b3b9dda27e96
topic_type:
- apiref
ms.openlocfilehash: 446de663d437c950f3a9be968e7dcbe8d25ed2b0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717223"
---
# <a name="icorprofilercallbackunmanagedtomanagedtransition-method"></a>ICorProfilerCallback::UnmanagedToManagedTransition 方法

通知探查器已发生从非托管代码到托管代码的转换。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT UnmanagedToManagedTransition(  
    [in] FunctionID functionId,  
    [in] COR_PRF_TRANSITION_REASON reason);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 中正在调用的函数的 ID。  
  
 `reason`  
 中一个 [COR_PRF_TRANSITION_REASON](cor-prf-transition-reason-enumeration.md) 枚举的值，该值指示是否由于从非托管代码调用托管代码而发生了转换，或是否是由托管函数调用的非托管函数返回。  
  
## <a name="remarks"></a>注解  

 如果的值 `reason` 为 COR_PRF_TRANSITION_RETURN 且不为 `functionId` null，则函数 ID 将为非托管函数的，并且永远不会使用实时 (JIT) 编译器进行编译。 非托管函数有一些与之关联的基本信息，如名称和某些元数据。  
  
 如果 COR_PRF_TRANSITION_CALL 的值 `reason` ，则被调用的函数可能 (即，) 尚未进行 JIT 编译的托管函数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
- [ManagedToUnmanagedTransition 方法](icorprofilercallback-managedtounmanagedtransition-method.md)
- [在 c + + 中使用显式 PInvoke (DllImport 特性) ](/cpp/dotnet/using-explicit-pinvoke-in-cpp-dllimport-attribute)
- [使用 c + + 互操作 (隐式 PInvoke) ](/cpp/dotnet/using-cpp-interop-implicit-pinvoke)
