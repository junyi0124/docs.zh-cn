---
title: ICorProfilerInfo4::GetCodeInfo3 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo4.GetCodeInfo3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo4::GetCodeInfo3
helpviewer_keywords:
- GetCodeInfo3 method, ICorProfilerInfo4 interface [.NET Framework profiling]
- ICorProfilerInfo4::GetCodeInfo3 method [.NET Framework profiling]
ms.assetid: bb8c105e-4d9a-4684-8c05-ed6909cc1b8c
topic_type:
- apiref
ms.openlocfilehash: 44454b87bb656d2a4ad6e597ae001f64222b905f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697761"
---
# <a name="icorprofilerinfo4getcodeinfo3-method"></a>ICorProfilerInfo4::GetCodeInfo3 方法

获取本机代码的范围，该代码与指定函数的 JIT 重新编译版本相关联。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCodeInfo3(  
    [in]  FunctionID functionID,  
    [in]  ReJITID reJitId,  
    [in]  ULONG32 cCodeInfos,  
    [out] ULONG32 *pcCodeInfos,  
    [out, size_is(cCodeInfos), length_is(*pcCodeInfos)]  
    COR_PRF_CODE_INFO codeInfos[]);  
```  
  
## <a name="parameters"></a>参数  

 `functionID`  
 [in] 与本机代码关联的函数的 ID。  
  
 `reJitId`  
 [in] JIT 重新编译的函数的标识。  
  
 `cCodeInfos`  
 [in] `codeInfos` 数组的大小。  
  
 `pcCodeInfos`  
 弄指向可用 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构总数的指针。  
  
 `codeInfos`  
 [out] 调用方提供的缓冲区。 返回此方法后，它包含一个 `COR_PRF_CODE_INFO` 结构数组，每个结构描述一个本机代码块。  
  
## <a name="remarks"></a>注解  

 `GetCodeInfo3`方法类似于[GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md)，只不过它将获取包含指定 IP 地址的函数的 JIT 重新编译的 ID。  
  
> [!NOTE]
> `GetCodeInfo3` 可以触发垃圾回收，而 [GetCodeInfo2](icorprofilerinfo2-getcodeinfo2-method.md) 将不会触发垃圾回收。 有关详细信息，请参阅 [CORPROF_E_UNSUPPORTED_CALL_SEQUENCE](corprof-e-unsupported-call-sequence-hresult.md) HRESULT。  
  
 范围按公共中间语言 (CIL) 偏移递增的顺序进行排序。  
  
 `GetCodeInfo3`返回后，必须验证 `codeInfos` 缓冲区大小是否足以包含所有[COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)的结构。 为此，请将 `cCodeInfos` 的值和 `cchName` 参数的值进行比较。 如果 `cCodeInfos` 除以 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构的大小小于 `pcCodeInfos` ，则分配更大的 `codeInfos` 缓冲区， `cCodeInfos` 用新的、更大的大小更新，然后再次调用 `GetCodeInfo3` 。  
  
 或者，可以先用长度为零的 `codeInfos` 缓冲区调用 `GetCodeInfo3` 以获取正确的缓冲区大小。 然后，可以将 `codeInfos` 缓冲区大小设置为中返回的值 `pcCodeInfos` ，再乘以 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构的大小，然后 `GetCodeInfo3` 再次调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetCodeInfo2 方法](icorprofilerinfo2-getcodeinfo2-method.md)
- [ICorProfilerInfo4 接口](icorprofilerinfo4-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
