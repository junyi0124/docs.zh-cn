---
title: EMemoryAvailable 枚举
ms.date: 03/30/2017
api_name:
- EMemoryAvailable
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EMemoryAvailable
helpviewer_keywords:
- EMemoryAvailable enumeration [.NET Framework hosting]
ms.assetid: 38e72a06-dbed-473b-a59b-7e0b3ea4f2af
topic_type:
- apiref
ms.openlocfilehash: 6a8765bfd62a2e6543661804ab8d009ce19f8813
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724307"
---
# <a name="ememoryavailable-enumeration"></a>EMemoryAvailable 枚举

包含指示计算机上的可用物理内存量的值。 这些值从逻辑上映射到 Windows API 中从函数返回的高和低内存的事件 `CreateMemoryResourceNotification` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    eMemoryAvailableLow     = 1,  
    eMemoryAvailableNeutral = 2,  
    eMemoryAvailableHigh    = 3
} EMemoryAvailable;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`eMemoryAvailableHigh`|有大量物理内存可用。|  
|`eMemoryAvailableLow`|物理内存非常少。|  
|`eMemoryAvailableNeutral`|可用物理内存为中性。|  
  
## <a name="remarks"></a>注解  

 使用 [ICLRMemoryNotificationCallback：： OnMemoryNotification](iclrmemorynotificationcallback-onmemorynotification-method.md) 方法调用，此值由主机传递到公共语言运行时 (CLR) 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
