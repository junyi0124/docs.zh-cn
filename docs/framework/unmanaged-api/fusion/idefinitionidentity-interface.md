---
title: IDefinitionIdentity 接口
ms.date: 03/30/2017
api_name:
- IDefinitionIdentity
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- IDefinitionIdentity
helpviewer_keywords:
- IDefinitionIdentity interface [.NET Framework fusion]
ms.assetid: ce5ba888-5fbe-4efd-91cf-f0ff94d8428b
topic_type:
- apiref
ms.openlocfilehash: 4f08fbbf9c8be16dff092327713e731c5aa14661
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732859"
---
# <a name="idefinitionidentity-interface"></a>IDefinitionIdentity 接口

表示定义当前作用域中的应用程序的代码的唯一签名。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|`IDefinitionIdentity::Clone`|获取一个接口指针，该指针指向与 `IDefinitionIdentity` 此相同的新对象 `IDefinitionIdentity` ，但指定的特性更改除外。|  
|`IDefinitionIdentity::EnumAttributes`|获取一个指向 [IEnumIDENTITY_ATTRIBUTE](ienumidentity-attribute-interface.md) 对象的接口指针，该对象包含与此关联的特性 `IDefinitionIdentity` 。|  
|`IDefinitionIdentity::GetAttribute`|获取指定命名空间中具有指定名称的属性的值。|  
|`IDefinitionIdentity::SetAttribute`|将指定命名空间中具有指定名称的特性设置为指定值。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 隔离。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [合成接口](fusion-interfaces.md)
