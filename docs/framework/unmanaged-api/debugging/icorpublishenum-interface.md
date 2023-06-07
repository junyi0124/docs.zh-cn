---
title: ICorPublishEnum 接口
ms.date: 03/30/2017
api_name:
- ICorPublishEnum
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorPublishEnum
helpviewer_keywords:
- ICorPublishEnum interface [.NET Framework debugging]
ms.assetid: 76a136b5-e444-417a-8ade-f1596d597dc7
topic_type:
- apiref
ms.openlocfilehash: 492d4b727ce507340fec47d30a791aa49d0cecb6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95693341"
---
# <a name="icorpublishenum-interface"></a>ICorPublishEnum 接口

用作枚举器的抽象基接口，这些枚举器用于发布有关进程和应用程序域的信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Clone 方法](icorpublishenum-clone-method.md)|创建此 `ICorPublishEnum` 对象的一个副本。|  
|[GetCount 方法](icorpublishenum-getcount-method.md)|获取枚举中的项数。|  
|[Reset 方法](icorpublishenum-reset-method.md)|将光标移到枚举的开头。|  
|[Skip 方法](icorpublishenum-skip-method.md)|按指定的项数在枚举中向前移动光标。|  
  
## <a name="remarks"></a>注解  

 以下枚举器派生自 `ICorPublishEnum` ：  
  
- [ICorPublishAppDomainEnum](icorpublishappdomainenum-interface.md)  
  
- [ICorPublishProcessEnum](icorpublishprocessenum-interface.md)  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CorPub，CorPub  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorpubPublish Coclass](corpubpublish-coclass.md)
- [调试接口](debugging-interfaces.md)
