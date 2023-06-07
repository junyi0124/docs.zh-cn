---
title: ICLRMemoryNotificationCallback 接口
ms.date: 03/30/2017
api_name:
- ICLRMemoryNotificationCallback
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMemoryNotificationCallback
helpviewer_keywords:
- ICLRMemoryNotificationCallback interface [.NET Framework hosting]
ms.assetid: 873639e2-4837-4568-83b3-4493e67e4174
topic_type:
- apiref
ms.openlocfilehash: 5762a354bec485cb382b5460dfd2a337f79bee1a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689532"
---
# <a name="iclrmemorynotificationcallback-interface"></a>ICLRMemoryNotificationCallback 接口

允许主机使用类似于 Win32 函数的方法来报告内存压力情况 `CreateMemoryResourceNotification` 。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[OnMemoryNotification 方法](iclrmemorynotificationcallback-onmemorynotification-method.md)| (CLR) 计算机上的内存负载通知公共语言运行时。|  
  
## <a name="remarks"></a>注解  

 主机使用 `ICLRMemoryNotificationCallback` 接口请求 CLR 释放内存资源。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostMemoryManager 接口](ihostmemorymanager-interface.md)
- [承载接口](hosting-interfaces.md)
