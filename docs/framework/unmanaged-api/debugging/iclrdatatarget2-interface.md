---
title: ICLRDataTarget2 接口
ms.date: 03/30/2017
api_name:
- ICLRDataTarget2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget2
helpviewer_keywords:
- ICLRDataTarget2 interface [.NET Framework debugging]
ms.assetid: 94249397-861b-4294-a538-cf01466a66d3
topic_type:
- apiref
ms.openlocfilehash: dee5108439610b67c3397cebcd8ee5f84d4eacea
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723631"
---
# <a name="iclrdatatarget2-interface"></a>ICLRDataTarget2 接口

数据访问服务层用于处理目标进程中的虚拟内存区域的 [ICLRDataTarget](iclrdatatarget-interface.md) 的子类。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[AllocVirtual 方法](iclrdatatarget2-allocvirtual-method.md)|在目标进程的地址空间中分配内存。|  
|[FreeVirtual 方法](iclrdatatarget2-freevirtual-method.md)|释放以前在目标进程的地址空间中分配的内存。|  
  
## <a name="remarks"></a>注解  

 API 客户端（即调试器）必须针对特定的目标进程实现此接口。 例如，活动进程的实现将不同于内存转储的。 目标可能不支持对其内存区域的修改。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
- [调试接口](debugging-interfaces.md)
