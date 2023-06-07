---
title: IHostSecurityManager::OpenThreadToken 方法
ms.date: 03/30/2017
api_name:
- IHostSecurityManager.OpenThreadToken
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IHostSecurityManager::OpenThreadToken
helpviewer_keywords:
- IHostSecurityManager::OpenThreadToken method [.NET Framework hosting]
- OpenThreadToken method [.NET Framework hosting]
ms.assetid: d5999052-8bf0-4a9e-8621-da6284406b18
topic_type:
- apiref
ms.openlocfilehash: 30ec8cc8bbbd6d49f89cd67371c3326c0cb0df9a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95680607"
---
# <a name="ihostsecuritymanageropenthreadtoken-method"></a>IHostSecurityManager::OpenThreadToken 方法

打开与当前正在执行的线程关联的自由访问令牌。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT OpenThreadToken (  
    [in]  DWORD    dwDesiredAccess,
    [in]  BOOL     bOpenAsSelf,
    [out] HANDLE   *phThreadToken  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwDesiredAccess`  
 中访问值的掩码，指定请求的线程标记访问类型。 这些值是在 Win32 函数中定义的 `OpenThreadToken` 。 请求的访问类型将与令牌的自由访问控制列表 (DACL) ，以确定要授予或拒绝的访问权限的类型。  
  
 `bOpenAsSelf`  
 [in] `true` 指定应使用调用线程的进程的安全上下文进行访问检查; `false` 若为，则指定应使用调用线程本身的安全上下文来执行访问检查。 如果线程正在模拟客户端，安全上下文可以是客户端进程的安全上下文。  
  
 `phThreadToken`  
 弄指向新打开的访问令牌的指针。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|`OpenThreadToken` 已成功返回。|  
|HOST_E_CLRNOTAVAILABLE| (CLR) 的公共语言运行时未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
|HOST_E_TIMEOUT|调用超时。|  
|HOST_E_NOT_OWNER|调用方不拥有该锁。|  
|HOST_E_ABANDONED|已阻止的线程或纤程正在等待某个事件时，该事件被取消。|  
|E_FAIL|发生未知的灾难性故障。 当方法返回 E_FAIL 时，CLR 在该进程内将不再可用。 对宿主方法的后续调用会返回 HOST_E_CLRNOTAVAILABLE。|  
  
## <a name="remarks"></a>注解  

 `IHostSecurityManager::OpenThreadToken` 的行为类似于具有相同名称的对应 Win32 函数，只不过 Win32 函数允许调用方将句柄传入任意线程，同时 `IHostSecurityManager::OpenThreadToken` 仅打开与调用线程关联的标记。  
  
 `HANDLE`类型不符合 COM 要求，也就是说，其大小特定于操作系统，并需要自定义封送处理。 因此，此令牌仅在该进程内的 CLR 和主机之间使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IHostSecurityContext 接口](ihostsecuritycontext-interface.md)
- [IHostSecurityManager 接口](ihostsecuritymanager-interface.md)
