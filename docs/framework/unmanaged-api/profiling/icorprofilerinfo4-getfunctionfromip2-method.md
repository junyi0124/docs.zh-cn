---
title: ICorProfilerInfo4::GetFunctionFromIP2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetFunctionFromIP2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetFunctionFromIP2
helpviewer_keywords:
- ICorProfilerInfo4::GetFunctionFromIP2 method [.NET Framework profiling]
- GetFunctionFromIP2 method, ICorProfilerInfo4 interface [.NET Framework profiling]
ms.assetid: 46ff70f4-13e9-40a0-802a-0a40abcfc6a0
topic_type:
- apiref
ms.openlocfilehash: a522df8abfba1c5837e3136f966935ff1f56d8d2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721538"
---
# <a name="icorprofilerinfo4getfunctionfromip2-method"></a>ICorProfilerInfo4::GetFunctionFromIP2 方法

将托管代码指令指针映射到函数的 JIT 重新编译版本。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFunctionFromIP2(  
    [in]  LPCBYTE    ip,  
    [out] FunctionID *pFunctionId,  
    [out] ReJITID *pReJitId);  
```  
  
## <a name="parameters"></a>参数  

 `ip`  
 中托管代码中的指令指针。  
  
 `pFunctionId`  
 弄函数 ID。  
  
 `pReJitId`  
 弄函数的 JIT 重新编译版本的标识。  
  
## <a name="remarks"></a>注解  

 `GetFunctionFromIP2` 类似于 `GetFunctionFromIP` ，只不过它会获取 JIT 重新编译的 ID，而不是包含指定 IP 地址的函数的函数 ID。  
  
> [!NOTE]
> `GetFunctionFromIP2` 可以触发垃圾回收，而 `GetFunctionFromIP` 不会。  有关详细信息，请参阅 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE HRESULT](corprof-e-unsupported-call-sequence-hresult.md)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
