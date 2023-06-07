---
title: ICLRAssemblyIdentityManager::GetProbingAssembliesFromReference 方法
ms.date: 03/30/2017
api_name:
- ICLRAssemblyIdentityManager.GetProbingAssembliesFromReference
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAssemblyIdentityManager::GetProbingAssembliesFromReference
helpviewer_keywords:
- ICLRAssemblyIdentityManager::GetProbingAssembliesFromReference method [.NET Framework hosting]
- GetProbingAssembliesFromReference method [.NET Framework hosting]
ms.assetid: aec05744-e8d4-44c6-b4a8-e583229ac34e
topic_type:
- apiref
ms.openlocfilehash: 263058131e63205aa37f81847ed647944fef7540
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731379"
---
# <a name="iclrassemblyidentitymanagergetprobingassembliesfromreference-method"></a>ICLRAssemblyIdentityManager::GetProbingAssembliesFromReference 方法

获取具有指定标识类型的程序集所引用的程序集标识的 [ICLRProbingAssemblyEnum](iclrprobingassemblyenum-interface.md) 枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetProbingAssembliesFromReference (  
    [in] DWORD   dwMachineType,  
    [in] DWORD   dwFlags,  
    [in] LPCWSTR pwzReferenceIdentity,  
    [out] ICLRProbingAssemblyEnum **ppProbingAssemblyEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwMachineType`  
 中一个有效的值，该值指定处理器体系结构（如 WinNT 中所定义）。  
  
 `dwFlags`  
 中提供用于将来的扩展性。 CLR_ASSEMBLY_IDENTITY_FLAGS_DEFAULT 是公共语言运行时的当前版本 (CLR) 支持的唯一值。  
  
 `pwzReferenceIdentity`  
 中不透明的程序集绑定标识，通常是从对 [ICLRAssemblyIdentityManager：： GetBindingIdentityFromFile](iclrassemblyidentitymanager-getbindingidentityfromfile-method.md) 或 [ICLRAssemblyIdentityManager：： GetBindingIdentityFromStream](iclrassemblyidentitymanager-getbindingidentityfromstream-method.md) 方法的调用返回的。  
  
 `ppProbingAssemblyEnum`  
 弄一个指向 `ICLRProbingAssemblyEnum` 枚举数的接口指针，该枚举数包含对由标识的程序集所引用的程序集的引用 `pwzReferenceIdentity` 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 如果方法返回 E_FAIL，则 CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [ICLRProbingAssemblyEnum 接口](iclrprobingassemblyenum-interface.md)
