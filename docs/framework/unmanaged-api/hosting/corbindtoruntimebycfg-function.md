---
title: CorBindToRuntimeByCfg 函数
ms.date: 03/30/2017
api_name:
- CorBindToRuntimeByCfg
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntimeByCfg
helpviewer_keywords:
- CorBindToRuntimeByCfg function [.NET Framework hosting]
ms.assetid: ded1e492-a782-4185-9c66-709e421c1782
topic_type:
- apiref
ms.openlocfilehash: d319382b577844a804c3e4562676491a15de5f63
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673769"
---
# <a name="corbindtoruntimebycfg-function"></a>CorBindToRuntimeByCfg 函数

使用从 XML 文件中读取的版本信息，将公共语言运行时 (CLR) 加载到进程中。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CorBindToRuntimeByCfg (  
    [in]  IStream     *pCfgStream,  
    [in]  DWORD        reserved,  
    [in]  DWORD        startupFlags,  
    [in]  REFCLSID     rclsid,  
    [in]  REFIID       riid,
    [out] LPVOID FAR*  ppv  
);  
```  
  
## <a name="parameters"></a>参数  

 `pCfgStream`  
 中指向 `IStream` 读取 XML 文件的对象的指针。  
  
 `reserved`  
 中保留供将来使用。 使用 0 (零) 作为值。  
  
 `startupFlags`  
 中 [STARTUP_FLAGS](startup-flags-enumeration.md) 枚举的一个值，该值指定 CLR 的启动行为。  
  
 `rclsid`  
 中 `CLSID` 用于实现 [ICorRuntimeHost](icorruntimehost-interface.md) 或 [ICLRRuntimeHost](iclrruntimehost-interface.md) 接口的 coclass 的。 支持的值为 CLSID_CorRuntimeHost 或 CLSID_CLRRuntimeHost。  
  
 `riid`  
 中 `IID` `ICorRuntimeHost` 或 `ICLRRuntimeHost` 接口的。 支持的值为 IID_ICorRuntimeHost 或 IID_ICLRRuntimeHost。  
  
 `ppv`  
 弄指向返回的接口的地址的指针。  
  
## <a name="remarks"></a>注解  

 XML 文件的格式将建模为标准应用程序配置文件。 有关 XML 文件的详细信息，请参阅 [配置文件架构](../../configure-apps/file-schema/index.md)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorBindToCurrentRuntime 函数](corbindtocurrentruntime-function.md)
- [CorBindToRuntime 函数](corbindtoruntime-function.md)
- [CorBindToRuntimeEx 函数](corbindtoruntimeex-function.md)
- [CorBindToRuntimeHost 函数](corbindtoruntimehost-function.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
