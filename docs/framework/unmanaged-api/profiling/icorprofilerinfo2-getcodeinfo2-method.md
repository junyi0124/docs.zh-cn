---
title: ICorProfilerInfo2::GetCodeInfo2 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetCodeInfo2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetCodeInfo2
helpviewer_keywords:
- ICorProfilerInfo2::GetCodeInfo2 method [.NET Framework profiling]
- GetCodeInfo2 method [.NET Framework profiling]
ms.assetid: 532da6ee-7f0a-401b-a61e-fc47ec235d2e
topic_type:
- apiref
ms.openlocfilehash: e88fe1b3c93ca278d0e64a5eb3274c86bd8f0f6d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727128"
---
# <a name="icorprofilerinfo2getcodeinfo2-method"></a>ICorProfilerInfo2::GetCodeInfo2 方法

获取与指定 `FunctionID` 关联的本机代码的范围。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCodeInfo2(  
    [in]  FunctionID functionID,  
    [in]  ULONG32 cCodeInfos,  
    [out] ULONG32 *pcCodeInfos,  
    [out, size_is(cCodeInfos), length_is(*pcCodeInfos)]  
    COR_PRF_CODE_INFO codeInfos[]);  
```  
  
## <a name="parameters"></a>参数  

 `functionID`  
 [in] 与本机代码关联的函数的 ID。  
  
 `cCodeInfos`  
 [in] `codeInfos` 数组的大小。  
  
 `pcCodeInfos`  
 弄指向可用 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构总数的指针。  
  
 `codeInfos`  
 [out] 调用方提供的缓冲区。 返回此方法后，它包含一个 `COR_PRF_CODE_INFO` 结构数组，每个结构描述一个本机代码块。  
  
## <a name="remarks"></a>注解  

 范围按 Microsoft 中间语言 (MSIL) 偏移递增的顺序进行排序。  
  
 返回 `GetCodeInfo2` 后，必须验证 `codeInfos` 缓冲区大小是否足以包含所有 `COR_PRF_CODE_INFO` 结构。 为此，请将 `cCodeInfos` 的值和 `cchName` 参数的值进行比较。 如果由 `COR_PRF_CODE_INFO` 结构大小划分的 `cCodeInfos` 值小于 `pcCodeInfos` 值，请分配更大的 `codeInfos` 缓冲区，将 `cCodeInfos` 更新为新的更大大小，并再次调用 `GetCodeInfo2`。  
  
 或者，可以先用长度为零的 `codeInfos` 缓冲区调用 `GetCodeInfo2` 以获取正确的缓冲区大小。 然后可将 `codeInfos` 缓冲区大小设置为 `pcCodeInfos` 中返回的，乘以 `COR_PRF_CODE_INFO` 结构大小的值，并再次调用 `GetCodeInfo2`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetCodeInfo3 方法](icorprofilerinfo4-getcodeinfo3-method.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
