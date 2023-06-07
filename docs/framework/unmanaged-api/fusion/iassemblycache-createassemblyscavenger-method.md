---
title: IAssemblyCache::CreateAssemblyScavenger 方法
ms.date: 03/30/2017
api_name:
- IAssemblyCache.CreateAssemblyScavenger
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IAssemblyCache::CreateAssemblyScavenger
helpviewer_keywords:
- CreateAssemblyScavenger method [.NET Framework fusion]
- IAssemblyCache::CreateAssemblyScavenger method [.NET Framework fusion]
ms.assetid: e8bb98f1-e477-45d2-8956-ba404137cd2d
topic_type:
- apiref
ms.openlocfilehash: 1d4bddca6f1fe881099e80bd9269ab392b1207fa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697020"
---
# <a name="iassemblycachecreateassemblyscavenger-method"></a>IAssemblyCache::CreateAssemblyScavenger 方法

保留供合成技术内部使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateAssemblyScavenger (  
    [out] IUnknown **ppUnkReserved  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppUnkReserved`  
 弄返回的 `IUnknown` 指针。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyCache 接口](iassemblycache-interface.md)
