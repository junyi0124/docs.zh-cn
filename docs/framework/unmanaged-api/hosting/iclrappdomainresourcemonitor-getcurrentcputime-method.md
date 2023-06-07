---
title: ICLRAppDomainResourceMonitor::GetCurrentCpuTime 方法
ms.date: 03/30/2017
api_name:
- ICLRAppDomainResourceMonitor.GetCurrentCpuTime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAppDomainResourceMonitor::GetCurrentCpuTime
helpviewer_keywords:
- ICLRAppDomainResourceMonitor::GetCurrentCpuTime method [.NET Framework hosting]
- GetCurrentCpuTime method [.NET Framework hosting]
ms.assetid: ebc9cc33-fcd6-4cae-9ecb-ea21c51874e6
topic_type:
- apiref
ms.openlocfilehash: a0b966e85bedcbef622aba2f6b181b98e0950e01
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95700673"
---
# <a name="iclrappdomainresourcemonitorgetcurrentcputime-method"></a>ICLRAppDomainResourceMonitor::GetCurrentCpuTime 方法

获取自应用程序域创建以来在当前应用程序域中执行时所有线程已使用的总处理器时间。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCurrentCpuTime([in]  DWORD dwAppDomainId,  
                          [out] ULONGLONG* pMilliseconds);  
```  
  
## <a name="parameters"></a>参数  

 `dwAppDomainId`  
 中请求的应用程序域的 ID。  
  
 `pMilliseconds`  
 弄一个指针，指向自创建应用程序域后，所有线程在当前应用程序域中执行时所使用的总处理器时间。 此参数可以为 `null`。  
  
## <a name="return-value"></a>返回值  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|COR_E_APPDOMAINUNLOADED|应用程序域已卸载或不存在。|  
|E_FAIL|未启用应用程序域资源监视。<br /><br /> -或-<br /><br /> 所有其他失败。|  
  
## <a name="remarks"></a>注解  

 此方法是托管属性的非托管等效项 <xref:System.AppDomain.MonitoringTotalProcessorTime%2A?displayProperty=nameWithType> 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [ICLRAppDomainResourceMonitor 接口](iclrappdomainresourcemonitor-interface.md)
- [承载接口](hosting-interfaces.md)
- [应用程序域资源监视](../../../standard/garbage-collection/app-domain-resource-monitoring.md)
- [承载](index.md)
