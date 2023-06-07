---
title: ICLRAppDomainResourceMonitor::GetCurrentSurvived 方法
ms.date: 03/30/2017
api_name:
- ICLRAppDomainResourceMonitor.GetCurrentSurvived
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRAppDomainResourceMonitor::GetCurrentSurvived
helpviewer_keywords:
- ICLRAppDomainResourceMonitor::GetCurrentSurvived method [.NET Framework hosting]
- GetCurrentSurvived method [.NET Framework hosting]
ms.assetid: 392e9009-40ef-40e3-ad4d-7ce93a989e78
topic_type:
- apiref
ms.openlocfilehash: eba9caece91e369cd46aed652b559ace49c77725
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704898"
---
# <a name="iclrappdomainresourcemonitorgetcurrentsurvived-method"></a>ICLRAppDomainResourceMonitor::GetCurrentSurvived 方法

获取当前应用程序域引用的最后一个完整的、阻止的垃圾回收和所引用的字节数。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT STDMETHODCALLTYPE GetCurrentSurvived(  
             [in]  DWORD dwAppDomainId,  
             [out] ULONGLONG *pAppDomainBytesSurvived,  
             [out] ULONGLONG *pTotalBytesSurvived);  
```  
  
## <a name="parameters"></a>参数  

 `dwAppDomainId`  
 中请求的应用程序域的 ID。  
  
 `pAppDomainBytesSurvived`  
 弄一个指针，该指针指向此应用程序域的上次垃圾回收后保留下来的字节数。 完成回收后，此数字准确且完整。 暂时回收后，此编号可能不完整。 此参数可以为 `null`。  
  
 `pRuntimeBytesSurvived`  
 弄指向上次垃圾回收后保留下来的总字节数的指针。 在完整回收后，此数字表示托管堆中保留的字节数。 暂时回收后，此数字表示暂时生成中保留的字节数。 此参数可以为 `null`。  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 HRESULT 以及表示方法失败的 HRESULT 错误。  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|该方法已成功完成。|  
|COR_E_APPDOMAINUNLOADED|应用程序域已卸载或不存在。|  
  
## <a name="remarks"></a>注解  

 仅在完整的、阻止的垃圾回收后更新统计信息。即，包含所有代并在收集过程中停止应用程序的集合。 例如， <xref:System.GC.Collect?displayProperty=nameWithType> 方法重载执行完全阻塞的集合。 并发垃圾回收在后台发生，不会阻止应用程序。  
  
 `GetCurrentSurvived`方法是托管属性的非托管等效项 <xref:System.AppDomain.MonitoringSurvivedMemorySize%2A?displayProperty=nameWithType> 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>请参阅

- [ICLRAppDomainResourceMonitor 接口](iclrappdomainresourcemonitor-interface.md)
- [应用程序域资源监视](../../../standard/garbage-collection/app-domain-resource-monitoring.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
