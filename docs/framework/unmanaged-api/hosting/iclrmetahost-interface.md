---
title: ICLRMetaHost 接口
ms.date: 03/30/2017
api_name:
- ICLRMetaHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRMetaHost
helpviewer_keywords:
- ICLRMetaHost interface [.NET Framework hosting]
ms.assetid: c627fcdd-fc4f-4b1c-8e91-df8536f627d8
topic_type:
- apiref
ms.openlocfilehash: 75c8d550e572795a291f4639f9f28bd5214ff188
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714011"
---
# <a name="iclrmetahost-interface"></a>ICLRMetaHost 接口

提供一些方法，这些方法基于公共语言运行时的版本号返回特定版本的公共语言运行时 () ，列出所有已安装的 Clr，列出在指定进程中加载的所有运行时，发现编译程序集所用的 CLR 版本，退出使用干净运行时关闭的进程，以及查询旧的 API 绑定。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnumerateInstalledRuntimes 方法](iclrmetahost-enumerateinstalledruntimes-method.md)|返回一个枚举，该枚举包含计算机上安装的每个 CLR 版本的有效 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口指针。|  
|[EnumerateLoadedRuntimes 方法](iclrmetahost-enumerateloadedruntimes-method.md)|返回一个枚举，该枚举包含给定进程中加载的每个 CLR 的有效 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口指针。 此方法取代了 [GetVersionFromProcess](getversionfromprocess-function.md)。|  
|[ExitProcess 方法](iclrmetahost-exitprocess-method.md)|尝试正常关闭所有加载的运行时，然后终止进程。 取代 [CorExitProcess](corexitprocess-function.md) 函数。|  
|[GetRuntime 方法](iclrmetahost-getruntime-method.md)|获取与特定 CLR 版本相对应的 [ICLRRuntimeInfo](iclrruntimeinfo-interface.md) 接口。 此方法取代了与[STARTUP_LOADER_SAFEMODE](startup-flags-enumeration.md)标志一起使用的[CorBindToRuntimeEx](corbindtoruntimeex-function.md)函数。|  
|[GetVersionFromFile 方法](iclrmetahost-getversionfromfile-method.md)|获取在元) 数据中存储的程序集的原始 .NET Framework 编译版本 (（给定其文件路径）。 此方法取代了 [GetFileVersion](getfileversion-function.md)。|  
|[QueryLegacyV2RuntimeBinding 方法](iclrmetahost-querylegacyv2runtimebinding-method.md)|返回一个接口，该接口表示已绑定旧式激活策略的运行时，例如通过使用 `useLegacyV2RuntimeActivationPolicy` [ \<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)配置文件项上的特性，通过直接使用旧的激活 Api 或通过调用[ICLRRuntimeInfo：： BindAsLegacyV2Runtime](iclrruntimeinfo-bindaslegacyv2runtime-method.md)方法来实现。|  
|[RequestRuntimeLoadedNotification 方法](iclrmetahost-requestruntimeloadednotification-method.md)|当首次加载 CLR 版本但尚未启动时，保证对指定函数指针的回调。 此方法取代 [LockClrVersion](lockclrversion-function.md)|  
  
## <a name="remarks"></a>注解  

 获取此接口实例的唯一方法是调用 [CLRCreateInstance](clrcreateinstance-function.md) 函数，如下所示：  
  
```cpp  
ICLRMetaHost *pMetaHost = NULL;  
HRESULT hr = CLRCreateInstance(CLSID_CLRMetaHost,  
                   IID_ICLRMetaHost, (LPVOID*)&pMetaHost);  
```  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [承载](index.md)
