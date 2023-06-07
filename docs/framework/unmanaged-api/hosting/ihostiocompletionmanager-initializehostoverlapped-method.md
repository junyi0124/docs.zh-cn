---
title: IHostIoCompletionManager::InitializeHostOverlapped 方法
ms.date: 03/30/2017
api_name:
- IHostIoCompletionManager.InitializeHostOverlapped
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostIoCompletionManager::InitializeHostOverlapped
helpviewer_keywords:
- IHostIoCompletionManager::InitializeHostOverlapped method [.NET Framework hosting]
- InitializeHostOverlapped method [.NET Framework hosting]
ms.assetid: c35199bf-bc47-4901-b467-4e8a37644bbb
topic_type:
- apiref
ms.openlocfilehash: 397dbbeb0b85cb549a8b5917f977ecb13b3d6539
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720212"
---
# <a name="ihostiocompletionmanagerinitializehostoverlapped-method"></a>IHostIoCompletionManager::InitializeHostOverlapped 方法

向宿主提供初始化任何自定义数据以追加到 `OVERLAPPED` 用于异步 i/o 请求的 Win32 结构的机会。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT InitializeHostOverlapped (  
    [in] void* pvOverlapped  
);  
```  
  
## <a name="parameters"></a>参数  

 `pvOverlapped`  
 中指向要 `OVERLAPPED` 包含在 i/o 请求中的 Win32 结构的指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`InitializeHostOverlapped` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用于分配请求的资源。|  
  
## <a name="remarks"></a>注解  

 Windows 平台函数使用 `OVERLAPPED` 结构来存储异步 i/o 请求的状态。 CLR 将调用 `InitializeHostOverlapped` 方法，以为宿主向宿主追加自定义数据的机会 `OVERLAPPED` 。  
  
> [!IMPORTANT]
> 若要从自定义数据块开始，主机必须将偏移量设置为 `OVERLAPPED` () 结构的大小 `sizeof(OVERLAPPED)` 。  
  
 E_OUTOFMEMORY 的返回值指示宿主未能初始化其自定义数据。 在这种情况下，CLR 将报告错误并导致调用失败。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRIoCompletionManager 接口](iclriocompletionmanager-interface.md)
- [GetHostOverlappedSize 方法](ihostiocompletionmanager-gethostoverlappedsize-method.md)
- [IHostIoCompletionManager 接口](ihostiocompletionmanager-interface.md)
