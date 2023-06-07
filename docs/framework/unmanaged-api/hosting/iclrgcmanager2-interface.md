---
title: ICLRGCManager2 接口
ms.date: 03/30/2017
api_name:
- ICLRGCManager2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRGCManager2
helpviewer_keywords:
- ICLRGCManager2 interface [.NET Framework hosting]
ms.assetid: 4b5ffd7b-9ad7-41cd-9bba-34030ae3da7e
topic_type:
- apiref
ms.openlocfilehash: d2873b5e1f8229e8a16dfaacf1c9737ac47405bb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672794"
---
# <a name="iclrgcmanager2-interface"></a>ICLRGCManager2 接口

提供允许主机与公共语言运行时的垃圾回收系统交互的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[SetGCStartupLimitsEx 方法](iclrgcmanager2-setgcstartuplimitsex-method.md)|设置垃圾回收段的大小以及垃圾回收系统的第0代的最大大小。 启用0代和大于的段大小 `DWORD` 。|  
  
## <a name="remarks"></a>注解  

 此接口继承自 [ICLRGCManager 接口](iclrgcmanager-interface.md)。  
  
 公共语言运行时 (CLR) 利用托管类型实现其垃圾回收机制 <xref:System.GC> 。 有关垃圾回收系统的详细信息，请参阅 [垃圾](../../../standard/garbage-collection/index.md)回收。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [自动内存管理](../../../standard/automatic-memory-management.md)
- [COR_GC_STATS 结构](cor-gc-stats-structure.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [.NET Framework 4 和 4.5 中添加的 CLR 承载接口](clr-hosting-interfaces-added-in-the-net-framework-4-and-4-5.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
