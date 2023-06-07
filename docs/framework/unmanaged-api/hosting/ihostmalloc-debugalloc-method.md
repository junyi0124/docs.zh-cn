---
title: IHostMAlloc::DebugAlloc 方法
ms.date: 03/30/2017
api_name:
- IHostMAlloc.DebugAlloc
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostMAlloc::DebugAlloc
helpviewer_keywords:
- DebugAlloc method [.NET Framework hosting]
- IHostMAlloc::DebugAlloc method [.NET Framework hosting]
ms.assetid: 0bfbc527-bea2-43ce-b041-69186f4440dd
topic_type:
- apiref
ms.openlocfilehash: 49de0398dd32970bd8ceb0fd24178e75df5d14b1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713482"
---
# <a name="ihostmallocdebugalloc-method"></a>IHostMAlloc::DebugAlloc 方法

请求宿主从堆分配指定内存量，并另外跟踪内存的分配位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DebugAlloc (  
    [in]  SIZE_T  cbSize,
    [in]  EMemoryCriticalLevel dwCriticalLevel,
    [in]  char*   pszFileName,
    [in]  int     iLineNo,
    [out] void**  ppMem  
);  
```  
  
## <a name="parameters"></a>参数  

 `cbSize`  
 中当前内存分配请求的大小（以字节为单位）。  
  
 `dwCriticalLevel`  
 中 [EMemoryCriticalLevel](ememorycriticallevel-enumeration.md) 值之一，指示分配失败的影响。  
  
 `pszFileName`  
 中正在调试的可执行文件的代码文件。  
  
 `iLineNo`  
 中 `pszFileName` 请求分配的行号。  
  
 `ppMem`  
 弄指向分配的内存的指针; 如果请求无法完成，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`DebugAlloc` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
|E_OUTOFMEMORY|没有足够的内存可用来完成分配请求。|  
  
## <a name="remarks"></a>注解  

 CLR 通过调用[IHostMemoryManager：： CreateMalloc](ihostmemorymanager-createmalloc-method.md)方法获取指向[IHostMalloc](ihostmalloc-interface.md)实例的接口指针。 `DebugAlloc` 允许运行时获取代码文件信息，以便在调试期间使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
- [IHostMalloc 接口](ihostmalloc-interface.md)
