---
title: ICorRuntimeHost::NextDomain 方法
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.NextDomain
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::NextDomain
helpviewer_keywords:
- ICorRuntimeHost::NextDomain method [.NET Framework hosting]
- NextDomain method [.NET Framework hosting]
ms.assetid: fe07a05b-f6d6-44b5-ab01-b9a6eb15c350
topic_type:
- apiref
ms.openlocfilehash: 598c46d50d7b4a67c1b2c0d844c9b12deb12a428
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671364"
---
# <a name="icorruntimehostnextdomain-method"></a>ICorRuntimeHost::NextDomain 方法

获取一个接口指针，该指针指向枚举中的下一个域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NextDomain (  
    [in] HCORENUM hEnum,  
    [out] void** pAppDomain  
);  
```  
  
## <a name="parameters"></a>参数  

 `hEnum`  
 中通过调用 [EnumDomains](icorruntimehost-enumdomains-method.md)获取的枚举器。  
  
 `pAppDomain`  
 弄指向 <xref:System._AppDomain?displayProperty=nameWithType> 表示枚举中下一个域的类型的接口指针; 如果不存在其他域，则为 null。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|操作成功。|  
|S_FALSE|操作未能完成，或者枚举中没有更多的域。|  
|E_FAIL|发生了未知的灾难性故障。 如果某个方法返回 E_FAIL，则公共语言运行时 (CLR) 在该进程中不再可用。 对任何宿主 Api 的后续调用都会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- <xref:System._AppDomain>
- <xref:System.AppDomain>
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
