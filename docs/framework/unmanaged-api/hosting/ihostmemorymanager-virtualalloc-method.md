---
title: IHostMemoryManager::VirtualAlloc 方法
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.VirtualAlloc
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::VirtualAlloc
helpviewer_keywords:
- VirtualAlloc method [.NET Framework hosting]
- IHostMemoryManager::VirtualAlloc method [.NET Framework hosting]
ms.assetid: 4dff3646-a050-4bd9-ac31-fe307e8637ec
topic_type:
- apiref
ms.openlocfilehash: a2deabc5f1c7ea0f42b6d8ec3944d984854ae571
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731275"
---
# <a name="ihostmemorymanagervirtualalloc-method"></a>IHostMemoryManager::VirtualAlloc 方法

用作相应 Win32 函数的逻辑包装。 的 Win32 实现 `VirtualAlloc` 保留或提交调用进程的虚拟地址空间中的页面区域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT VirtualAlloc (  
    [in]  void*   pAddress,  
    [in]  SIZE_T  dwSize,  
    [in]  DWORD   flAllocationType,  
    [in]  DWORD   flProtect,  
    [in]  EMemoryCriticalLevel dwCriticalLevel,  
    [out] void**  ppMem  
);  
```  
  
## <a name="parameters"></a>参数  

 `pAddress`  
 中指向要分配的区域的起始地址的指针。  
  
 `dwSize`  
 中区域的大小（以字节为单位）。  
  
 `flAllocationType`  
 中内存分配的类型。  
  
 `flProtect`  
 中要分配的页面区域的内存保护。  
  
 `dwCriticalLevel`  
 中指示分配失败的影响的 [EMemoryCriticalLevel](ememorycriticallevel-enumeration.md) 值。  
  
 `ppMem`  
 弄指向分配的内存的起始地址的指针; 如果无法满足请求，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`VirtualAlloc` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的可用内存来完成分配请求|  
  
## <a name="remarks"></a>注解  

 可以通过调用在进程的地址空间中保留区域 `VirtualAlloc` 。 `pAddress`参数包含所需内存块的起始地址。 此参数通常设置为 null。 操作系统会保留可用于进程的免费地址范围记录。 如果 `pAddress` 值为 null，则指示系统在其认为合适的位置保留区域。 或者，您可以为内存块提供特定的起始地址。 在这两种情况下，output 参数 `ppMem` 将作为指向已分配内存的指针返回。 函数本身返回 HRESULT 值。  
  
 Win32 `VirtualAlloc` 函数没有 `ppMem` 参数，而是返回指向分配的内存的指针。 有关详细信息，请参阅 Windows 平台文档。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
