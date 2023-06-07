---
title: CorGenericParamAttr 枚举
ms.date: 03/30/2017
api_name:
- CorGenericParamAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorGenericParamAttr
helpviewer_keywords:
- CorGenericParamAttr enumeration [.NET Framework metadata]
ms.assetid: 36c76266-71d8-48dc-bd89-54943fa659c1
topic_type:
- apiref
ms.openlocfilehash: ea50430c3ae6cef9b47880bcb8ad969f62ce9c39
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704903"
---
# <a name="corgenericparamattr-enumeration"></a>CorGenericParamAttr 枚举

包含描述 <xref:System.Type> 泛型类型参数的值，这些值在对 [IMetaDataEmit2：:D efinegenericparam](imetadataemit2-definegenericparam-method.md)的调用中使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorGenericParamAttr {  
  
    gpVarianceMask                     =   0x0003,  
    gpNonVariant                       =   0x0000,
    gpCovariant                        =   0x0001,  
    gpContravariant                    =   0x0002,  
  
    gpSpecialConstraintMask            =   0x001C,  
    gpNoSpecialConstraint              =   0x0000,  
    gpReferenceTypeConstraint          =   0x0004,
    gpNotNullableValueTypeConstraint   =   0x0008,  
    gpDefaultConstructorConstraint     =   0x0010  
  
} CorGenericParamAttr;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`gpVarianceMask`|参数变体仅适用于接口和委托的泛型参数。|  
|`gpNonVariant`|指示缺少方差。|  
|`gpCovariant`|指示协方差。|  
|`gpContravariant`|指示逆变。|  
|`gpSpecialConstraintMask`|特殊约束可应用于任何 <xref:System.Type> 参数。|  
|`gpNoSpecialConstraint`|指示没有约束适用于 <xref:System.Type> 参数。|  
|`gpReferenceTypeConstraint`|指示 <xref:System.Type> 参数必须为引用类型。|  
|`gpNotNullableValueTypeConstraint`|指示 <xref:System.Type> 参数必须是不能为 null 值的值类型。|  
|`gpDefaultConstructorConstraint`|指示 <xref:System.Type> 参数必须包含不带任何参数的默认公共构造函数。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
