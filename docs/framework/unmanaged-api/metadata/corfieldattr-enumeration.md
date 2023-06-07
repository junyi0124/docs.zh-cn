---
title: CorFieldAttr 枚举
ms.date: 03/30/2017
api_name:
- CorFieldAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorFieldAttr
helpviewer_keywords:
- CorFieldAttr enumeration [.NET Framework metadata]
ms.assetid: 6ae2c4be-212c-4e74-9288-40a11dc26522
topic_type:
- apiref
ms.openlocfilehash: 4e40f684cc1578672cb8ff474972ce9cdc39efb2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718821"
---
# <a name="corfieldattr-enumeration"></a>CorFieldAttr 枚举

包含一些值，用于描述字段的相应元数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorFieldAttr {  
  
    fdFieldAccessMask           =   0x0007,  
    fdPrivateScope              =   0x0000,  
    fdPrivate                   =   0x0001,  
    fdFamANDAssem               =   0x0002,  
    fdAssembly                  =   0x0003,  
    fdFamily                    =   0x0004,  
    fdFamORAssem                =   0x0005,  
    fdPublic                    =   0x0006,  
  
    fdStatic                    =   0x0010,  
    fdInitOnly                  =   0x0020,  
    fdLiteral                   =   0x0040,  
    fdNotSerialized             =   0x0080,  
  
    fdSpecialName               =   0x0200,  
  
    fdPinvokeImpl               =   0x2000,  
  
    fdReservedMask              =   0x9500,  
    fdRTSpecialName             =   0x0400,  
    fdHasFieldMarshal           =   0x1000,  
    fdHasDefault                =   0x8000,  
    fdHasFieldRVA               =   0x0100  
  
} CorFieldAttr;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`fdFieldAccessMask`|指定辅助功能信息。|  
|`fdPrivateScope`|指定该字段不能被引用。|  
|`fdPrivate`|指定该字段只能由其父类型访问。|  
|`fdFamANDAssem`|指定该字段可由其程序集中的派生类访问。|  
|`fdAssembly`|指定该字段可由其程序集中的所有类型进行访问。|  
|`fdFamily`|指定该字段只能由其类型和派生类访问。|  
|`fdFamORAssem`|指定该字段可由派生类和其程序集中的所有类型进行访问。|  
|`fdPublic`|指定该字段可由具有此范围可见性的所有类型进行访问。|  
|`fdStatic`|指定该字段是其类型的成员而不是实例成员。|  
|`fdInitOnly`|指定字段在初始化之后不能更改。|  
|`fdLiteral`|指定字段值是编译时常量。|  
|`fdNotSerialized`|指定当字段的类型为 "远程" 时不对其进行序列化。|  
|`fdSpecialName`|指定该字段为特殊字段，并且其名称描述了操作方法。|  
|`fdPinvokeImpl`|指定通过 PInvoke 转发字段实现。|  
|`fdReservedMask`|保留供公共语言运行时内部使用。|  
|`fdRTSpecialName`|指定公共语言运行时元数据内部 Api 应检查名称的编码。|  
|`fdHasFieldMarshal`|指定该字段包含封送处理信息。|  
|`fdHasDefault`|指定该字段具有默认值。|  
|`fdHasFieldRVA`|指定该字段具有相对虚拟地址。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
