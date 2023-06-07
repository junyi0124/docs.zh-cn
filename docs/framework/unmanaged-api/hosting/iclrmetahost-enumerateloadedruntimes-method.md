---
title: ICLRMetaHost::EnumerateLoadedRuntimes 方法
ms.date: 03/30/2017
api_name:
- ICLRMetaHost.EnumerateLoadedRuntimes
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost::EnumerateLoadedRuntimes
helpviewer_keywords:
- EnumerateLoadedRuntimes method [.NET Framework hosting]
- ICLRMetaHost::EnumerateLoadedRuntimes method [.NET Framework hosting]
ms.assetid: 22fc0a3f-dce4-4766-9a3c-9fab15f4b4ca
topic_type:
- apiref
ms.openlocfilehash: 98184dd6ea16df066905039b028acd689ff3f290
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730430"
---
# <a name="iclrmetahostenumerateloadedruntimes-method"></a>ICLRMetaHost::EnumerateLoadedRuntimes 方法

返回一个枚举，其中包含给定进程中加载 (CLR) 的每个版本的公共语言运行时的有效 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口指针。 此方法取代了 [GetVersionFromProcess](getversionfromprocess-function.md) 函数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateLoadedRuntimes (  
    [in] HANDLE hndProcess,  
    [out, retval] IEnumUnknown **ppEnumerator  
);  
```  
  
## <a name="parameters"></a>参数  

 `hndProcess`  
 中要检查加载的运行时的进程的句柄。  
  
 `ppEnumerator`  
 弄 <xref:Microsoft.VisualStudio.OLE.Interop.IEnumUnknown> 与进程加载的每个 CLR 相对应的 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口的枚举。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`ppEnumerator` 为 null。|  
  
## <a name="remarks"></a>注解  

 此方法列出所有加载的运行时，即使它们是用弃用的函数（如 [CorBindToRuntime](corbindtoruntime-function.md)）加载的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRMetaHost 接口](iclrmetahost-interface.md)
- [承载](index.md)
