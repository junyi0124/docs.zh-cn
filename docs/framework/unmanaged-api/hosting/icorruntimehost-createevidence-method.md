---
title: ICorRuntimeHost::CreateEvidence 方法
ms.date: 03/30/2017
api_name:
- ICorRuntimeHost.CreateEvidence
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICorRuntimeHost::CreateEvidence
helpviewer_keywords:
- CreateEvidence method [.NET Framework hosting]
- ICorRuntimeHost::CreateEvidence method [.NET Framework hosting]
ms.assetid: e235ea80-b84c-4442-a4c3-fc96c25a8eb9
topic_type:
- apiref
ms.openlocfilehash: 6627fce519934177aefd26a612e5b00ca1941d02
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95715662"
---
# <a name="icorruntimehostcreateevidence-method"></a>ICorRuntimeHost::CreateEvidence 方法

获取类型的接口指针 <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> ，该指针允许主机创建要传递给 [CreateDomain](icorruntimehost-createdomain-method.md) 或 [CreateDomainEx](icorruntimehost-createdomainex-method.md) 方法的安全证据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateEvidence (  
    [out] IUnknown** pEvidence  
);  
```  
  
## <a name="parameters"></a>参数  

 `pEvidence`  
 弄一个接口指针，指向 <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> 用于创建安全证据的实例。 此指针的类型为 `IUnknown` ，因此调用方通常应 `QueryInterface` 在此接口上调用以获取指向的指针 <xref:System.Security.Principal.IIdentity?displayProperty=nameWithType> 。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|操作成功。|  
|S_FALSE|操作未能完成。|  
|E_FAIL|发生了未知的灾难性故障。 如果某个方法返回 E_FAIL，则公共语言运行时 (CLR) 在该进程中不再可用。 对任何宿主 Api 的后续调用都会返回 HOST_E_CLRNOTAVAILABLE。|  
|HOST_E_CLRNOTAVAILABLE|CLR 未加载到进程中，或 CLR 处于无法运行托管代码或成功处理调用的状态。|  
  
## <a name="remarks"></a>注解  

 此方法返回一个不能从本机代码中填充的空集合。 应 <xref:System.Security.Policy.Evidence> 改用方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：** 1.0、1。1  
  
## <a name="see-also"></a>另请参阅

- <xref:System._AppDomain>
- <xref:System.AppDomain>
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
