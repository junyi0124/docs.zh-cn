---
title: ICorPublishProcess::EnumAppDomains 方法
ms.date: 03/30/2017
api_name:
- ICorPublishProcess.EnumAppDomains
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishProcess::EnumAppDomains
helpviewer_keywords:
- EnumAppDomains method [.NET Framework debugging]
- ICorPublishProcess::EnumAppDomains method [.NET Framework debugging]
ms.assetid: 7da621fc-e7d0-4c00-9439-5c93619d7414
topic_type:
- apiref
ms.openlocfilehash: 2acf8fb507ab617e066a31c9c2657b1ef0d18e47
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95693276"
---
# <a name="icorpublishprocessenumappdomains-method"></a>ICorPublishProcess::EnumAppDomains 方法

获取此 [ICorPublishProcess](icorpublishprocess-interface.md)引用的进程中的应用程序域的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumAppDomains (  
    [out] ICorPublishAppDomainEnum   **ppEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 弄一个指针，指向允许在此进程中通过应用程序域集合进行迭代的 [ICorPublishAppDomainEnum](icorpublishappdomainenum-interface.md) 实例的地址。  
  
## <a name="remarks"></a>注解  

 应用程序域的列表基于在调用方法时存在的应用程序域的快照 `EnumAppDomains` 。 可以多次调用此方法来创建新的最新列表。 此方法的后续调用将不会影响现有列表。  
  
 如果进程已终止， `EnumAppDomains` 则将失败，并且 HRESULT 值为 CORDBG_E_PROCESS_TERMINATED。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub，CorPub  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorPublishProcess 接口](icorpublishprocess-interface.md)
