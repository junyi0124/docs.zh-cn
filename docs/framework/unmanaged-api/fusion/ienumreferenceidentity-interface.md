---
title: IEnumReferenceIdentity 接口
ms.date: 03/30/2017
api_name:
- IEnumReferenceIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IEnumReferenceIdentity
helpviewer_keywords:
- IEnumReferenceIdentity interface [.NET Framework fusion]
ms.assetid: a17b3155-7216-4e16-8c9f-abce21f549e7
topic_type:
- apiref
ms.openlocfilehash: bea357fe9a154ffb8f69228c7332c026dc2759e9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728962"
---
# <a name="ienumreferenceidentity-interface"></a>IEnumReferenceIdentity 接口

用作对象集合的枚举器 `IReferenceIdentity` 。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`IEnumReferenceIdentity::Clone`|获取一个接口指针，该指针指向 `IEnumReferenceIdentity` 包含与此相同的成员的新 `IEnumReferenceIdentity` 。|  
|`IEnumReferenceIdentity::Next`|`IReferenceIdentity`从当前位置开始，获取指定数目的对象。|  
|`IEnumReferenceIdentity::Reset`|将指令指针移到此的开头 `IEnumReferenceIdentity` 。|  
|`IEnumReferenceIdentity::Skip`|从当前位置开始，将指令指针向前移动指定数量的元素。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
- [IReferenceIdentity 接口](ireferenceidentity-interface.md)
