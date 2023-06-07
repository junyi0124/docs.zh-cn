---
title: ICorPublishAppDomainEnum 接口
ms.date: 03/30/2017
api_name:
- ICorPublishAppDomainEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishAppDomainEnum
helpviewer_keywords:
- ICorPublishAppDomainEnum interface [.NET Framework debugging]
ms.assetid: bb798c56-042e-475d-a245-b6fac36d0c07
topic_type:
- apiref
ms.openlocfilehash: 5b5a901bef779948467cfcc3d71a1fcd057c1aeb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95693705"
---
# <a name="icorpublishappdomainenum-interface"></a>ICorPublishAppDomainEnum 接口

[ICorPublishEnum](icorpublishenum-interface.md)接口的子类，它提供了用于遍历进程中当前存在的[ICorPublishAppDomain](icorpublishappdomain-interface.md)对象的集合的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Next 方法](icorpublishappdomainenum-next-method.md)|`ICorPublishAppDomain`从当前位置开始，获取集合中指定数量的实例。|  
  
## <a name="remarks"></a>注解  

 `ICorPublishAppDomainEnum`接口实现抽象接口的方法[ICorPublishEnum](icorpublishenum-interface.md)。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub，CorPub  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [CorpubPublish Coclass](corpubpublish-coclass.md)
