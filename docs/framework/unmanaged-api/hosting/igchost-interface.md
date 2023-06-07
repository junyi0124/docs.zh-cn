---
title: IGCHost 接口
ms.date: 03/30/2017
api_name:
- IGCHost
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IGCHost
helpviewer_keywords:
- IGCHost interface [.NET Framework hosting]
ms.assetid: 9ad70ffd-6963-4ab2-8c84-3d86c3fb8deb
topic_type:
- apiref
ms.openlocfilehash: 8965797321e68443c01d05f97d147f2320a76739
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724288"
---
# <a name="igchost-interface"></a>IGCHost 接口

提供一些方法，用于获取有关垃圾回收系统的信息并控制垃圾回收的某些方面。  
  
> [!NOTE]
> 从 .NET Framework 4.5 开始，你可以使用 [IGCHost2：： SetGCStartupLimitsEx](igchost2-setgcstartuplimitsex-method.md) 方法将垃圾回收段的大小和垃圾回收系统的第0代的最大大小设置为大于 `DWORD` [SetGCStartupLimits](igchost-setgcstartuplimits-method.md) 方法施加的限制的值。  
  
> [!NOTE]
> 此接口仅供专家使用。 如果使用不当，可能会影响应用程序的性能。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Collect 方法](igchost-collect-method.md)|为给定的生成强制执行回收，而不考虑当前垃圾回收的状态。|  
|[GetStats 方法](igchost-getstats-method.md)|获取垃圾收集系统的当前状态的统计信息。|  
|[GetThreadStats 方法](igchost-getthreadstats-method.md)|获取用于垃圾回收的每个线程的统计信息。|  
|[SetGCStartupLimits 方法](igchost-setgcstartuplimits-method.md)|设置第0代的段大小和最大大小。|  
|[SetVirtualMemLimit 方法](igchost-setvirtualmemlimit-method.md)|设置运行时的虚拟内存的最大大小。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost，GCHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [CorRuntimeHost 组件类](corruntimehost-coclass.md)
