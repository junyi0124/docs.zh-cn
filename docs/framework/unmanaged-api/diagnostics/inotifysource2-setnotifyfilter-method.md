---
title: INotifySource2::SetNotifyFilter 方法
ms.date: 03/30/2017
api_name:
- INotifySource2.SetNotifyFilter
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- INotifySource2::SetNotifyFilter
helpviewer_keywords:
- INotifySource2::SetNotifyFilter method [.NET Framework debugging]
- SetNotifyFilter method [.NET Framework debugging]
ms.assetid: 6351fc92-b126-4af6-9bf3-0a8ce92845fc
topic_type:
- apiref
ms.openlocfilehash: 99bce831405d722f1f1ca0ae56e60f95f2d905e8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719926"
---
# <a name="inotifysource2setnotifyfilter-method"></a>INotifySource2::SetNotifyFilter 方法

分配用于此源的通知筛选器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetNotifyFilter  
(  
    [in]  NOTIFY_FILTER   in_NotifyFilter,  
    [in]  USER_THREAD*    in_pUserThreadFilter  
);  
```  
  
## <a name="parameters"></a>参数  

 `in_NotifyFilter`  
 中 [NOTIFY_FILTER](notify-filter-enumeration.md) 枚举值的按位组合，这些值用于标识调试器 API 的回调。  
  
 `in_pUserThreadFilter`  
 中一个指针，指向用于标识调试器 API 的线程的 [USER_THREAD](user-thread-structure.md) 结构。  
  
## <a name="return-value"></a>返回值  

 如果方法成功，则 S_OK。  
  
## <a name="requirements"></a>要求  

 **标头：** ProtocolNotify2 .idl  
  
## <a name="see-also"></a>另请参阅

- [INotifySource2 接口](inotifysource2-interface.md)
- [INotifyConnection2 接口](inotifyconnection2-interface.md)
- [INotifySink2 接口](inotifysink2-interface.md)
