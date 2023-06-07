---
title: IGCHost::SetGCStartupLimits 方法
ms.date: 03/30/2017
api_name:
- IGCHost.SetGCStartupLimits
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- SetGCStartupLimits
helpviewer_keywords:
- SetGCStartupLimits method, IGCHost interface [.NET Framework hosting]
- IGCHost::SetGCStartupLimits method [.NET Framework hosting]
ms.assetid: cae53926-82ac-4d1d-b297-0bde0bd1bebb
topic_type:
- apiref
ms.openlocfilehash: 0eea9dba57886edfef13c31948a9cff94c6c1bfd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687868"
---
# <a name="igchostsetgcstartuplimits-method"></a>IGCHost::SetGCStartupLimits 方法

设置第0代的段大小和最大大小。  
  
> [!IMPORTANT]
> 从 .NET Framework 4.5 开始，可以 `DWORD` 使用 [IGCHost2：： SetGCStartupLimitsEx](igchost2-setgcstartuplimitsex-method.md) 方法将段大小和最大第0代大小设置为大于的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetGCStartupLimits (  
    [in] DWORD SegmentSize,  
    [in] DWORD MaxGen0Size  
);  
```  
  
## <a name="parameters"></a>参数  

 `SegmentSize`  
 中垃圾收集系统使用的段大小。  
  
 `MaxGen0Size`  
 中第0代的最大大小。  
  
## <a name="remarks"></a>注解  

 `SetGCStartupLimits`方法只能调用一次。 以后不能更改这些值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost，GCHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IGCHost 接口](igchost-interface.md)
