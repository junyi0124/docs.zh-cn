---
title: ICLRRuntimeInfo::IsLoaded 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.IsLoaded
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::IsLoaded
helpviewer_keywords:
- IsLoaded method [.NET Framework hosting]
- ICLRRuntimeInfo::IsLoaded method [.NET Framework hosting]
ms.assetid: fdc5a3a7-71ff-4025-99a1-59e4ee0bfe1b
topic_type:
- apiref
ms.openlocfilehash: 66ae74deba9ceab9d1ea6b2c0b96a87bf44f32ab
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714921"
---
# <a name="iclrruntimeinfoisloaded-method"></a>ICLRRuntimeInfo::IsLoaded 方法

指示公共语言运行时 (与 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口关联的 CLR) 是否已加载到进程中。 可以在不启动的情况下加载运行时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT IsLoaded(  
[in]  HANDLE hndProcess,  
[out, retval] BOOL *pbLoaded);  
```  
  
## <a name="parameters"></a>参数  

 `hndProcess`  
 中进程的句柄。  
  
 `pbLoaded`  
 [out] `true` 如果 CLR 加载到进程中，则为; 否则为。否则为 `false` 。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`pbLoaded` 为 null。|  
  
## <a name="remarks"></a>注解  

 此方法与以下函数和接口向后兼容：  
  
- [ICorRuntimeHost](icorruntimehost-interface.md) 接口在 .NET Framework 版本1宿主 API) 中 (。  
  
- .NET Framework 2.0 托管 API) 中 ([ICLRRuntimeHost](iclrruntimehost-interface.md)接口。  
  
- 弃用 `CorBindTo*` 的函数 (参阅 .NET Framework 2.0 托管 API) 中 [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md) 。  
  
 宿主可以调用某个弃用的函数（ `CorBindTo*` 如 [CorBindToRuntime](corbindtoruntime-function.md) 函数）来实例化特定版本的 CLR。 然后，宿主可以调用 [ICLRMetaHost：： GetRuntime](iclrmetahost-getruntime-method.md) 方法，并指定相同的版本号以获取 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口。  
  
 如果宿主在 `IsLoaded` 返回的 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口上调用方法，则 `pbLoaded` 返回 `true` ; 否则返回 `false` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
