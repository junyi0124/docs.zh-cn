---
title: ICorProfilerInfo::GetModuleInfo 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetModuleInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetModuleInfo
helpviewer_keywords:
- GetModuleInfo method [.NET Framework profiling]
- ICorProfilerInfo::GetModuleInfo method [.NET Framework profiling]
ms.assetid: 5a90d16f-7929-4987-8f83-a631becf564d
topic_type:
- apiref
ms.openlocfilehash: 863fa1bf50830bb46e5c2939c99fe1e15897ac3d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724125"
---
# <a name="icorprofilerinfogetmoduleinfo-method"></a>ICorProfilerInfo::GetModuleInfo 方法

给定模块 ID 后，将返回模块的文件名和模块的父程序集的 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetModuleInfo(  
    [in]  ModuleID   moduleId,  
    [out] LPCBYTE    *ppBaseLoadAddress,  
    [in]  ULONG      cchName,  
    [out] ULONG      *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR      szName[] ,  
    [out] AssemblyID *pAssemblyId);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 [in] 将为其检索信息的模块的 ID。  
  
 `ppBaseLoadAddress`  
 [out] 加载模块的基址。  
  
 `cchName`  
 [in] `szName` 返回缓冲区的长度（以字符为单位）。  
  
 `pcchName`  
 [out] 指向返回的模块文件名总字符长度的指针。  
  
 `szName`  
 [out] 调用方提供的宽字符缓冲区。 方法返回后，此缓冲区包含模块的文件名。  
  
 `pAssemblyId`  
 [out] 指向模块的父程序集的 ID 的指针。  
  
## <a name="remarks"></a>注解  

 对于动态模块，`szName` 参数是空字符串，并且基址是 0（零）。  
  
 尽管在 `GetModuleInfo` 模块的 ID 存在后可以调用方法，但在探查器接收到 [ICorProfilerCallback：： ModuleAttachedToAssembly](icorprofilercallback-moduleattachedtoassembly-method.md) 回调之前，父程序集的 ID 将不可用。  
  
 返回 `GetModuleInfo` 后，必须验证 `szName` 缓冲区的大小是否足够包含模块的完整文件名。 为此，请比较 `pcchName` 指向的值和 `cchName` 参数的值。 如果 `pcchName` 指向的值大于 `cchName`，请分配更大的 `szName` 缓冲区，并用新的、更大的大小更新 `cchName`，然后再次调用 `GetModuleInfo`。  
  
 或者，可以先用长度为零的 `szName` 缓冲区调用 `GetModuleInfo` 以获取正确的缓冲区大小。 然后，可将缓冲区大小设置为 `pcchName` 中返回的值，并再次调用 `GetModuleInfo`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
- [GetModuleInfo2 方法](icorprofilerinfo3-getmoduleinfo2-method.md)
