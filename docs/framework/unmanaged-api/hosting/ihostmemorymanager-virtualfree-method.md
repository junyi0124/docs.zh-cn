---
title: IHostMemoryManager::VirtualFree 方法
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.VirtualFree
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::VirtualFree
helpviewer_keywords:
- IHostMemoryManager::VirtualFree method [.NET Framework hosting]
- VirtualFree method [.NET Framework hosting]
ms.assetid: 1a436e89-eb28-4d15-bcf1-a072f86dbd99
topic_type:
- apiref
ms.openlocfilehash: be006afaf5966aa4e6d11c73b92004d676c97c7f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731262"
---
# <a name="ihostmemorymanagervirtualfree-method"></a>IHostMemoryManager::VirtualFree 方法

用作相应 Win32 函数的逻辑包装。 在 `VirtualFree` 调用进程的虚拟地址空间内释放、解除或释放和解除页区域的 Win32 实现。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT VirtualFree (  
    [in] LPVOID  lpAddress,  
    [in] SIZE_T  dwSize,  
    [in] DWORD   dwFreeType  
);  
```  
  
## <a name="parameters"></a>参数  

 `lpAddress`  
 中一个指针，指向要释放的虚拟内存页的基址。  
  
 `dwSize`  
 中要释放的区域的大小（以字节为单位）。  
  
 `dwFreeType`  
 中释放操作的类型。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`VirtualFree` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_INVALIDOPERATION|尝试释放未通过主机分配的内存。|  
  
## <a name="remarks"></a>注解  

 `VirtualFree``lpAddress`通过对[IHostMemoryManager：： VirtualAlloc](ihostmemorymanager-virtualalloc-method.md)函数的更早调用来释放与参数关联的虚拟内存页。 尝试释放未通过主机分配的内存应返回 HOST_E_INVALIDOPERATION。  
  
 语义与的 Win32 实现的语义相同 `VirtualFree` 。 有关详细信息，请参阅 Windows 平台文档。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
- [IHostMalloc 接口](ihostmalloc-interface.md)
