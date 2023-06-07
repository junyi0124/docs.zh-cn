---
title: CorBindToCurrentRuntime 函数
ms.date: 03/30/2017
api_name:
- CorBindToCurrentRuntime
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- HeaderDef
f1_keywords:
- CorBindToCurrentRuntime
helpviewer_keywords:
- CorBindToCurrentRuntime function [.NET Framework hosting]
ms.assetid: 6105c13e-d9cd-44d2-a95a-924e042830c7
topic_type:
- apiref
ms.openlocfilehash: 4a8ab6e1aeedef5b821fc977387b8039f54edd64
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95682492"
---
# <a name="corbindtocurrentruntime-function"></a>CorBindToCurrentRuntime 函数

通过使用存储在 XML 文件中的版本信息，将 (CLR) 中的公共语言运行时加载到进程中。 XML 文件的格式将建模为标准应用程序配置文件。 有关配置文件的详细信息，请参阅[配置文件架构](../../configure-apps/file-schema/index.md)。  
  
 此函数已在 .NET Framework 4 中弃用。 请参阅将 [公共语言运行时加载到进程中](/previous-versions/dotnet/netframework-4.0/01918c6x(v=vs.100))。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CorBindToCurrentRuntime (  
    [in]  LPCWSTR   pwszFileName,  
    [in]  REFCLSID  rclsid,  
    [in]  REFIID    riid,  
    [out] LPVOID    *ppv  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwszFileName`  
 中指定要加载的 CLR 版本的应用程序配置文件的名称。 如果文件名不是完全限定的，则假定它与进行调用的可执行文件位于同一目录中。  
  
 配置文件的元素中的 version 特性描述了要加载的运行时的版本 [\<requiredRuntime>](../../configure-apps/file-schema/startup/requiredruntime-element.md) 。  
  
 如果未指定任何版本，或者如果 `<requiredRuntime>` 找不到该元素，则会加载计算机上安装的最新版本的 CLR。  
  
 `rclsid`  
 中 `CLSID` 用于实现 [ICorRuntimeHost](icorruntimehost-interface.md) 或 [ICLRRuntimeHost](iclrruntimehost-interface.md) 接口的 coclass 的。 支持的值为 CLSID_CorRuntimeHost 或 CLSID_CLRRuntimeHost。  
  
 `riid`  
 中 `IID` 你请求的接口的。 支持的值为 IID_ICorRuntimeHost 或 IID_ICLRRuntimeHost。  
  
 `ppv`  
 弄返回的接口指针。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorBindToRuntime 函数](corbindtoruntime-function.md)
- [CorBindToRuntimeByCfg 函数](corbindtoruntimebycfg-function.md)
- [CorBindToRuntimeEx 函数](corbindtoruntimeex-function.md)
- [CorBindToRuntimeHost 函数](corbindtoruntimehost-function.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
