---
title: ICLRRuntimeInfo 接口
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo
helpviewer_keywords:
- ICLRRuntimeInfo interface [.NET Framework hosting]
ms.assetid: 287e5ede-b3a7-4ef8-a756-4fca3f285a82
topic_type:
- apiref
ms.openlocfilehash: 9f485728ddb050abf815bf8ba26c69be9c909785
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714964"
---
# <a name="iclrruntimeinfo-interface"></a>ICLRRuntimeInfo 接口

提供一些方法，这些方法可返回有关特定公共语言运行时 (CLR) 的信息，包括版本、目录和加载状态。 此接口还提供了特定于运行时的功能，而无需初始化运行时。 它包括运行时相对 [LoadLibrary](iclrruntimeinfo-loadlibrary-method.md) 方法、运行时模块特定的 [GetProcAddress](iclrruntimeinfo-getprocaddress-method.md) 方法和通过 [GetInterface](iclrruntimeinfo-getinterface-method.md) 方法提供的运行时提供的接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[BindAsLegacyV2Runtime 方法](iclrruntimeinfo-bindaslegacyv2runtime-method.md)|为所有旧版 CLR 版本2激活策略决策绑定此运行时。|  
|[GetDefaultStartupFlags 方法](iclrruntimeinfo-getdefaultstartupflags-method.md)|获取 CLR 启动标志和主机配置文件。|  
|[GetInterface 方法](iclrruntimeinfo-getinterface-method.md)|将 CLR 加载到当前进程并返回运行时接口指针，如 [ICLRRuntimeHost](iclrruntimehost-interface.md)、 [ICLRStrongName](iclrstrongname-interface.md) 和 [IMetaDataDispenser](../metadata/imetadatadispenser-interface.md)。 此方法将取代所有 `CorBindTo*` 函数。|  
|[GetProcAddress 方法](iclrruntimeinfo-getprocaddress-method.md)|获取从与此接口关联的 CLR 导出的指定函数的地址。 此方法取代了 [GetRealProcAddress](getrealprocaddress-function.md) 方法。|  
|[GetRuntimeDirectory 方法](iclrruntimeinfo-getruntimedirectory-method.md)|获取与此接口关联的 CLR 的安装目录。 此方法取代了 [GetCORSystemDirectory](getcorsystemdirectory-function.md) 方法。|  
|[GetVersionString 方法](iclrruntimeinfo-getversionstring-method.md)|获取公共语言运行时 (CLR) 版本信息与给定的 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口相关联。 此方法取代了 [GetRequestedRuntimeInfo](getrequestedruntimeinfo-function.md) 和 [GetRequestedRuntimeVersion](getrequestedruntimeversion-function.md) 方法。|  
|[IsLoadable 方法](iclrruntimeinfo-isloadable-method.md)|指示是否可将与此接口关联的运行时加载到当前进程，并考虑可能已加载到进程中的其他运行时。|  
|[IsLoaded 方法](iclrruntimeinfo-isloaded-method.md)|指示与 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口关联的 CLR 是否已加载到进程中。|  
|[IsStarted 方法](iclrruntimeinfo-isstarted-method.md)|指示与 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口关联的 CLR 是否已启动。|  
|[LoadErrorString 方法](iclrruntimeinfo-loaderrorstring-method.md)|将 HRESULT 值转换为指定区域性的适当错误消息。 此方法取代了 [LoadStringRC](loadstringrc-function.md) 和 [LoadStringRCEx](loadstringrcex-function.md) 方法。|  
|[LoadLibrary 方法](iclrruntimeinfo-loadlibrary-method.md)|从 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口所表示的 CLR 的框架目录加载库。 此方法取代了 [LoadLibraryShim](loadlibraryshim-function.md) 方法。|  
|[SetDefaultStartupFlags 方法](iclrruntimeinfo-setdefaultstartupflags-method.md)|设置 CLR 启动标志和主机配置文件。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [承载](index.md)
