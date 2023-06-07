---
title: ICorProfilerInfo::GetAssemblyInfo 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetAssemblyInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- GetAssemblyInfo Method
helpviewer_keywords:
- GetAssemblyInfo method [.NET Framework profiling]
- ICorProfilerInfo::GetAssemblyInfo method [.NET Framework profiling]
ms.assetid: 7a3c97c3-1e31-47b1-bf23-386785c509c4
topic_type:
- apiref
ms.openlocfilehash: ff81da15b17ab0a7fbe62b08e358f65eed3edb71
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680260"
---
# <a name="icorprofilerinfogetassemblyinfo-method"></a>ICorProfilerInfo::GetAssemblyInfo 方法

接受程序集 ID，并返回此程序集的名称及其清单模块的 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAssemblyInfo(  
    [in]  AssemblyID  assemblyId,  
    [in]  ULONG       cchName,  
    [out] ULONG       *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR       szName[] ,  
    [out] AppDomainID *pAppDomainId,  
    [out] ModuleID    *pModuleId);  
```  
  
## <a name="parameters"></a>参数  

 `assemblyId`  
 [in] 程序集的标识符。  
  
 `cchName`  
 [in] `szName` 的长度（以字符为单位）。  
  
 `pcchName`  
 [out] 指向程序集名称的总字符长度的指针。  
  
 `szName`  
 [out] 调用方提供的宽字符缓冲区。 函数返回时将包含程序集名称。  
  
 `pAppDomainId`  
 [out] 指向包含程序集的应用程序域 ID 的指针。  
  
 `pModuleId`  
 [out] 指向程序集的清单模块 ID 的指针。  
  
## <a name="remarks"></a>注解  

 此方法返回后，必须验证 `szName` 缓冲区大小是否足以包含程序集全名。 为此，请比较 `pcchName` 指向的值和 `cchName` 参数的值。 如果 `pcchName` 指向的值大于 `cchName`，请分配更大的 `szName` 缓冲区，并用新的、更大的大小更新 `cchName`，然后再次调用 `GetAssemblyInfo`。  
  
 或者，可以先用长度为零的 `szName` 缓冲区调用 `GetAssemblyInfo` 以获取正确的缓冲区大小。 然后可根据 `pcchName` 中返回的值调整缓冲区大小，并再次调用 `GetAssemblyInfo`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
