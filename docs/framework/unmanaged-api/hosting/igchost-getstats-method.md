---
title: IGCHost::GetStats 方法
ms.date: 03/30/2017
api_name:
- IGCHost.GetStats
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- GetStats
helpviewer_keywords:
- GetStats method, IGCHost interface [.NET Framework hosting]
- IGCHost::GetStats method [.NET Framework hosting]
ms.assetid: c4ae022c-46ac-4f19-9ddd-09b955f19412
topic_type:
- apiref
ms.openlocfilehash: 7e664d88bf9f67e936e693b663f27ca490da13ed
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95670103"
---
# <a name="igchostgetstats-method"></a>IGCHost::GetStats 方法

获取垃圾收集系统的当前状态的统计信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetStats (  
    [in, out] COR_GC_STATS *pStats  
);  
```  
  
## <a name="parameters"></a>参数  

 `pStats`  
 [in，out]指向 [COR_GC_STATS](cor-gc-stats-structure.md) 结构的指针，该结构包含垃圾收集系统的当前状态的统计信息。  
  
## <a name="remarks"></a>注解  

 智能分配系统可以使用统计信息来帮助垃圾回收系统操作。 例如，在查看统计信息后，分配系统可能会确定它是否需要添加更多内存或强制集合。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** GCHost，GCHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IGCHost 接口](igchost-interface.md)
