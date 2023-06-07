---
title: IDefinitionAppId 接口
ms.date: 03/30/2017
api_name:
- IDefinitionAppId
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDefinitionAppId
helpviewer_keywords:
- IDefinitionAppId interface [.NET Framework fusion]
ms.assetid: e72f2550-bdec-4a20-a2f4-2e14847266c1
topic_type:
- apiref
ms.openlocfilehash: 1e6c42d8e74d2d3e7925c657c67832f662416e64
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673860"
---
# <a name="idefinitionappid-interface"></a>IDefinitionAppId 接口

表示定义当前作用域中的应用程序的代码的唯一标识符。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`IDefinitionAppId::get_Codebase`|获取表示此对象中的代码的格式化字符串 `IDefinitionAppId` 。|  
|`IDefinitionAppId::put_Codebase`|将此对象的代码设置 `IDefinitionAppId` 为指定的格式化字符串值。|  
|`IDefinitionAppId::EnumAppPath`|获取一个指向 [IEnumDefinitionIdentity](ienumdefinitionidentity-interface.md) 对象的接口指针，该对象包含当前应用程序路径中的程序集。|  
|`IDefinitionAppId::SetAppPath`|将当前范围中的程序集的应用程序路径设置为指定的 [IDefinitionIdentity](idefinitionidentity-interface.md) 对象所引用的值。|  
|`IDefinitionAppId::get_SubscriptionId`|获取一个指针，该指针指向此对象的订阅的标记标识符的字符串表示形式 `IDefinitionAppId` 。|  
|`IDefinitionAppId::put_SubscriptionId`|将此对象的订阅的标记标识符设置 `IDefinitionAppId` 为指定的字符串值。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
