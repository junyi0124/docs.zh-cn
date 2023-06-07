---
title: IReferenceAppId 接口
ms.date: 03/30/2017
api_name:
- IReferenceAppId
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IReferenceAppId
helpviewer_keywords:
- IReferenceAppId interface [.NET Framework fusion]
ms.assetid: 8eb9e565-f358-43ce-900e-a8f8a5aa6cfb
topic_type:
- apiref
ms.openlocfilehash: 9aa7173d81d84d9080d90b0890769ffeaee6a738
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728610"
---
# <a name="ireferenceappid-interface"></a>IReferenceAppId 接口

表示对当前范围内的应用程序的唯一标识符的引用。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`IReferenceAppId::get_CodeBase`|获取一个指针，该指针指向由此引用的应用程序的代码标识符的字符串表示形式 `IReferenceAppId` 。|  
|`IReferenceAppId::put_CodeBase`|设置由此引用的应用程序的代码标识符 `IReferenceAppId` 。|  
|`IReferenceAppId::EnumAppPath`|获取一个接口指针，该指针指向一个 `IEnumReferenceIdentity` 实例，该实例包含 `IReferenceIdentity` 表示此的成员的实例 `IReferenceAppId` 。|  
|`IReferenceAppId::get_SubscriptionId`|获取一个指针，该指针指向此的订阅的标记标识符的字符串表示形式 `IReferenceAppId` 。|  
|`IReferenceAppId::put_SubscriptionId`|将此订阅的标记标识符设置为 `IReferenceAppId` 指定的字符串值。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
- [IEnumReferenceIdentity 接口](ienumreferenceidentity-interface.md)
- [IReferenceIdentity 接口](ireferenceidentity-interface.md)
