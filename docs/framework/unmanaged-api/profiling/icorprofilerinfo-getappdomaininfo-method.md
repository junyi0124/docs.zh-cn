---
title: ICorProfilerInfo::GetAppDomainInfo 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetAppDomainInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetAppDomainInfo
helpviewer_keywords:
- ICorProfilerInfo::GetAppDomainInfo method [.NET Framework profiling]
- GetAppDomainInfo method [.NET Framework profiling]
ms.assetid: a6bf5a04-e03e-44f0-917a-96f6a6d3cc96
topic_type:
- apiref
ms.openlocfilehash: 62055a98197f5f8bd4cfc02e99891b83ef6341e9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680276"
---
# <a name="icorprofilerinfogetappdomaininfo-method"></a>ICorProfilerInfo::GetAppDomainInfo 方法

接受应用程序域 ID。 返回应用程序域名称和包含该域的进程的 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetAppDomainInfo(  
    [in]  AppDomainID appDomainId,  
    [in]  ULONG       cchName,  
    [out] ULONG       *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)]  
          WCHAR       szName[] ,  
    [out] ProcessID   *pProcessId);  
```  
  
## <a name="parameters"></a>参数  

 `appDomainId`  
 [in] 应用程序域的 ID。  
  
 `cchName`  
 [in] `szName` 返回缓冲区的长度（以字符为单位）。  
  
 `pcchName`  
 [out] 指向应用程序域名称的总字符长度的指针。  
  
 `szName`  
 [out] 调用方提供的宽字符缓冲区。 当方法返回时，`szName` 将包含整个或部分应用程序域名称。  
  
 `pProcessId`  
 [out] 指向包含应用程序域的进程的 ID 的指针。  
  
## <a name="remarks"></a>注解  

 此方法返回后，必须验证 `szName` 缓冲区是否足够大从而可包含应用程序域的完整名称。 为此，请比较 `pcchName` 指向的值和 `cchName` 参数的值。 如果 `pcchName` 指向的值大于 `cchName`，请分配更大的 `szName` 缓冲区，并用新的、更大的大小更新 `cchName`，然后再次调用 `GetAppDomainInfo`。  
  
 或者，可以先用长度为零的 `szName` 缓冲区调用 `GetAppDomainInfo` 以获取正确的缓冲区大小。 然后，可将缓冲区大小设置为 `pcchName` 中返回的值，并再次调用 `GetAppDomainInfo`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
