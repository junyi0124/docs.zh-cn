---
title: IHostMemoryManager 接口
ms.date: 03/30/2017
api_name:
- IHostMemoryManager
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMemoryManager
helpviewer_keywords:
- IHostMemoryManager interface [.NET Framework hosting]
ms.assetid: a945d439-3b34-4aa4-b575-8413dd7806ce
topic_type:
- apiref
ms.openlocfilehash: 0f71e4c45e43c2027b12998532f2b04401a51951
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731327"
---
# <a name="ihostmemorymanager-interface"></a>IHostMemoryManager 接口

提供一些方法，通过这些方法，公共语言运行时 (CLR) 通过主机进行虚拟内存请求，而不是使用标准 Win32 虚拟内存函数。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AcquiredVirtualAddressSpace 方法](ihostmemorymanager-acquiredvirtualaddressspace-method.md)|向宿主通知公共语言运行时 (CLR) 已从操作系统中获取指定内存。|  
|[CreateMAlloc 方法](ihostmemorymanager-createmalloc-method.md)|获取一个接口指针，该指针指向用于从主机创建的堆请求内存分配的 [IHostMAlloc](ihostmalloc-interface.md) 实例。|  
|[GetMemoryLoad 方法](ihostmemorymanager-getmemoryload-method.md)|获取主机报告的当前正使用的物理内存量。|  
|[NeedsVirtualAddressSpace 方法](ihostmemorymanager-needsvirtualaddressspace-method.md)|向宿主通知 CLR 将尝试使用指定的内存。|  
|[RegisterMemoryNotificationCallback 方法](ihostmemorymanager-registermemorynotificationcallback-method.md)|注册一个指针，该指针指向主机调用以通知 CLR 当前计算机上的当前内存负载的回调函数。|  
|[ReleasedVirtualAddressSpace 方法](ihostmemorymanager-releasedvirtualaddressspace-method.md)|向宿主通知 CLR 已使用指定的内存完成。|  
|[VirtualAlloc 方法](ihostmemorymanager-virtualalloc-method.md)|用作相应 Win32 函数的逻辑包装，该函数可在调用进程的虚拟地址空间中保留或提交页区域。|  
|[VirtualFree 方法](ihostmemorymanager-virtualfree-method.md)|用作相应 Win32 函数的逻辑包装，该函数释放、解除或释放和解除调用进程的虚拟地址空间中的页面区域。|  
|[VirtualProtect 方法](ihostmemorymanager-virtualprotect-method.md)|用作相应 Win32 函数的逻辑包装，该函数更改调用进程的虚拟地址空间中已提交页面区域的保护。|  
|[VirtualQuery 方法](ihostmemorymanager-virtualquery-method.md)|用作相应 Win32 函数的逻辑包装，它检索有关调用进程的虚拟地址空间中的一系列页面的信息。|  
  
## <a name="remarks"></a>注解  

 `IHostMemoryManager` 还为 CLR 提供一些方法，用于获取一个指针，通过该指针可以在堆上发出内存请求并在进程中获取由主机报告的内存压力级别。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMalloc 接口](ihostmalloc-interface.md)
- [承载接口](hosting-interfaces.md)
