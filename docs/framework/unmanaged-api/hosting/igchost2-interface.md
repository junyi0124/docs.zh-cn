---
title: IGCHost2 接口
ms.date: 03/30/2017
api_name:
- IGCHost2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IGCHost2
helpviewer_keywords:
- IGCHost2 interface [.NET Framework hosting]
ms.assetid: e5323fa4-18ac-424d-859d-a65a550d08d9
topic_type:
- apiref
ms.openlocfilehash: 7529ecd215d74eb0eedbec8b90eba367ed20d56f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687738"
---
# <a name="igchost2-interface"></a>IGCHost2 接口

提供一些方法，用于获取有关垃圾回收系统的信息并控制垃圾回收的某些方面。  
  
> [!NOTE]
> 对于新开发，建议改为使用 [ICLRGCManager2](iclrgcmanager2-interface.md) 接口。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[SetGCStartupLimitsEx 方法](igchost2-setgcstartuplimitsex-method.md)|设置第0代的段大小和最大大小。 启用0代和大于的段大小 `DWORD` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost，GCHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
- [CLR 承载接口](clr-hosting-interfaces.md)
- [CorRuntimeHost 组件类](corruntimehost-coclass.md)
