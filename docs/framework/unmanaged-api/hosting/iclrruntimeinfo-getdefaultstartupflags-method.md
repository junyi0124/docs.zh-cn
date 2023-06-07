---
title: ICLRRuntimeInfo::GetDefaultStartupFlags 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.GetDefaultStartupFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::GetDefaultStartupFlags
helpviewer_keywords:
- ICLRRuntimeInfo::GetDefaultStartupFlags method [.NET Framework hosting]
- GetDefaultStartupFlags method [.NET Framework hosting]
ms.assetid: 35c2173e-3b0b-4b2a-950d-e0a01c6df052
topic_type:
- apiref
ms.openlocfilehash: 2f828a3720f7313ee9cb851c6adae78bd5ea4fe8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732082"
---
# <a name="iclrruntimeinfogetdefaultstartupflags-method"></a>ICLRRuntimeInfo::GetDefaultStartupFlags 方法

获取启动标志和主机配置文件，该文件将用于启动运行时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetDefaultStartupFlags(  
     [out]  DWORD *pdwStartupFlags,  
     [out, size_is(*pcchHostConfigFile)] LPWSTR pwzHostConfigFile,  
     [in, out]  DWORD *pcchHostConfigFile);  
```  
  
## <a name="parameters"></a>参数  

 `pdwStartupFlags`  
 弄指向当前设置的主机启动标志的指针。  
  
 `pwzHostConfigFile`  
 弄一个指针，指向当前宿主配置文件的目录路径。  
  
 `pcchHostConfigFile`  
 [in，out]输入时， `pwzHostConfigFile` 为避免缓冲区溢出。 如果 `pwzHostConfigFile` 为 null，则该方法为预分配返回所需的大小 `pwzHostConfigFile` 。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定的 HRESULT 以及指示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
  
## <a name="remarks"></a>注解  

 此方法返回 (和) 的默认标志值 `STARTUP_CONCURRENT_GC` `NULL` ，或者通过对 [ICLRRuntimeInfo：： SetDefaultStartupFlags 方法](iclrruntimeinfo-setdefaultstartupflags-method.md)的前一次调用提供的值或任何 `CorBind*` 方法（如果它们绑定到此运行时）所设置的值。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
