---
title: ICLRProbingAssemblyEnum 接口
ms.date: 03/30/2017
api_name:
- ICLRProbingAssemblyEnum
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRProbingAssemblyEnum
helpviewer_keywords:
- ICLRProbingAssemblyEnum interface [.NET Framework hosting]
ms.assetid: e7d3ccab-b0f0-4872-8935-0ed72920171b
topic_type:
- apiref
ms.openlocfilehash: 6df08889af30542af5a128cbffc38a57ce640fde
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728922"
---
# <a name="iclrprobingassemblyenum-interface"></a>ICLRProbingAssemblyEnum 接口

提供一些方法，这些方法使宿主可以通过使用公共语言运行时 (CLR) 内部的程序集的标识信息来获取程序集的探测标识，无需创建或了解该标识。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Get 方法](iclrprobingassemblyenum-get-method.md)|获取指定索引处的程序集标识。|  
  
## <a name="remarks"></a>注解  

 方法（如 [ICLRAssemblyIdentityManager：： GetProbingAssembliesFromReference](iclrassemblyidentitymanager-getprobingassembliesfromreference-method.md) ）返回 `ICLRProbingAssemblyEnum` 实例。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [承载接口](hosting-interfaces.md)
