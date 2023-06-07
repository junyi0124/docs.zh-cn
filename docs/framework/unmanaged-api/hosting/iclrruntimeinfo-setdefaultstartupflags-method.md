---
title: ICLRRuntimeInfo::SetDefaultStartupFlags 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.SetDefaultStartupFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::SetDefaultStartupFlags
helpviewer_keywords:
- ICLRRuntimeInfo::SetDefaultStartupFlags method [.NET Framework hosting]
- SetDefaultStartupFlags method [.NET Framework hosting]
ms.assetid: 98ae174f-bff0-48f1-9e05-6cb63b451824
topic_type:
- apiref
ms.openlocfilehash: 8020db491c3b66be38a9f6cbcb7551721859dcd5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723111"
---
# <a name="iclrruntimeinfosetdefaultstartupflags-method"></a>ICLRRuntimeInfo::SetDefaultStartupFlags 方法

设置启动标志和将用于启动运行时的主机配置文件。 此方法取代了 `startupFlags` [CorBindToRuntimeEx](corbindtoruntimeex-function.md) 和 [CorBindToRuntimeHost](corbindtoruntimehost-function.md) 函数中的参数使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetDefaultStartupFlags(  
           [in]  DWORD dwStartupFlags,  
           [in]  LPCWSTR pwzHostConfigFile);  
```  
  
## <a name="parameters"></a>参数  

 `dwStartupFlags`  
 中要设置的主机启动标志。 使用与 [CorBindToRuntimeEx](corbindtoruntimeex-function.md) 和 [CorBindToRuntimeHost](corbindtoruntimehost-function.md) 函数相同的标志。  
  
 `pwzHostConfigFile`  
 中要设置的主机配置文件的目录路径。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定的 HRESULT 以及指示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
  
## <a name="remarks"></a>注解  

 多线程主机应同步对此方法的调用。 否则，线程 A 可能会 `SetStartupFlags` 在线程 b 完成对的调用 `SetStartupFlags` 和线程 b 启动运行时之前调用方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
