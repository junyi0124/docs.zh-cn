---
title: ICLRAssemblyIdentityManager::GetReferencedAssembliesFromFile 方法
ms.date: 03/30/2017
api_name:
- ICLRAssemblyIdentityManager.GetReferencedAssembliesFromFile
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAssemblyIdentityManager::GetReferencedAssembliesFromFile
helpviewer_keywords:
- ICLRAssemblyIdentityManager::GetReferenceAssembliesFromFile method [.NET Framework hosting]
- GetReferenceAssembliesFromFile method [.NET Framework hosting]
ms.assetid: eed63d31-d977-4c7d-9443-f9d37a2a7d81
topic_type:
- apiref
ms.openlocfilehash: 6b67ba9d022d94f51d7cc6a4645855f6b6ac3e19
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679308"
---
# <a name="iclrassemblyidentitymanagergetreferencedassembliesfromfile-method"></a>ICLRAssemblyIdentityManager::GetReferencedAssembliesFromFile 方法

获取一个 [ICLRReferenceAssemblyEnum](iclrreferenceassemblyenum-interface.md) 实例，该实例包含程序集在指定文件路径处引用的程序集的列表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetReferencedAssembliesFromFile (  
    [in]  LPCWSTR pwzFilePath,  
    [in]  DWORD   dwFlags,  
    [in]  ICLRAssemblyReferenceList   *pExcludeAssembliesList,  
    [out] ICLRReferenceAssemblyEnum  **ppReferenceEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzFilePath`  
 中要计算的程序集的路径。  
  
 `dwFlags`  
 中提供用于将来的扩展性。 CLR_ASSEMBLY_IDENTITY_FLAGS_DEFAULT 是公共语言运行时的当前版本 (CLR) 支持的唯一值。  
  
 `pExcludeAssembliesList`  
 中指向 [ICLRAssemblyReferenceList](iclrassemblyreferencelist-interface.md) 对象的指针，该对象表示应从中排除的程序集 `ppReferenceEnum` 。  
  
 `ppReferenceEnum`  
 弄指向对象地址的指针 `ICLRReferenceAssemblyEnum` ，该对象包含程序集引用的程序集的程序集标识数据，不包括由表示的程序集 `pwzFilePath` `pExcludeAssembliesList` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 如果方法返回 E_FAIL，则 CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 调用方可以选择从返回的列表中排除一组已知的程序集引用。 此集由 `pExcludeAssembliesList` 参数定义。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [ICLRReferenceAssemblyEnum 接口](iclrreferenceassemblyenum-interface.md)
