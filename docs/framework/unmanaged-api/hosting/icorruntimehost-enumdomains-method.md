---
title: ICorRuntimeHost::EnumDomains 方法
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.EnumDomains
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::EnumDomains
helpviewer_keywords:
- ICorRuntimeHost::EnumDomains method [.NET Framework hosting]
- EnumDomains method [.NET Framework hosting]
ms.assetid: 96b74995-0cde-4876-b6df-7fc164e6a5d1
topic_type:
- apiref
ms.openlocfilehash: f4338429dc24bf5196b92d3f18e73c98442f61e7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720657"
---
# <a name="icorruntimehostenumdomains-method"></a>ICorRuntimeHost::EnumDomains 方法

获取当前进程中的域的枚举数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumDomains (  
    [out] HCORENUM *hEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `hEnum`  
 弄域的枚举数。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|操作成功。|  
|S_FALSE|操作未能完成。|  
|E_FAIL|发生了未知的灾难性故障。 如果某个方法返回 E_FAIL，则公共语言运行时 (CLR) 在该进程中不再可用。 对任何宿主 Api 的后续调用都会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
