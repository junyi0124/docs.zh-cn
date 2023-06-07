---
title: IHostMemoryManager::VirtualQuery 方法
ms.date: 03/30/2017
api_name:
- IHostMemoryManager.VirtualQuery
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager::VirtualQuery
helpviewer_keywords:
- IHostMemoryManager::VirtualQuery method [.NET Framework hosting]
- VirtualQuery method [.NET Framework hosting]
ms.assetid: 757af1e6-b9e8-49e7-b5db-342be3aa205f
topic_type:
- apiref
ms.openlocfilehash: 6e3cb5bcec831f143d45f733c9e2f977390aade6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731236"
---
# <a name="ihostmemorymanagervirtualquery-method"></a>IHostMemoryManager::VirtualQuery 方法

用作相应 Win32 函数的逻辑包装。 的 Win32 实现 `VirtualQuery` 检索有关调用进程的虚拟地址空间中的页范围的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT VirtualQuery (  
    [in]  void*    lpAddress,  
    [out] void*    lpBuffer,  
    [in]  SIZE_T   dwLength,  
    [out] SIZE_T*  pResult  
);  
```  
  
## <a name="parameters"></a>参数  

 `lpAddress`  
 中指向要查询的虚拟内存中的地址的指针。  
  
 `lpBuffer`  
 弄指向结构的指针，该结构包含有关指定的内存区域的信息。  
  
 `dwLength`  
 中指向的缓冲区的大小（以字节为单位） `lpBuffer` 。  
  
 `pResult`  
 弄指向信息缓冲区返回的字节数的指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`VirtualQuery` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 `VirtualQuery` 提供有关调用进程的虚拟地址空间中的页范围的信息。 此实现将参数的值设置 `pResult` 为信息缓冲区中返回的字节数，并返回一个 HRESULT 值。 在 Win32 `VirtualQuery` 函数中，返回值为缓冲区大小。 有关详细信息，请参阅 Windows 平台文档。  
  
> [!IMPORTANT]
> 操作系统的实现 `VirtualQuery` 不会导致死锁，并且可以运行到完成，并在用户代码中挂起随机线程。 实现此方法的承载版本时要格外小心。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
