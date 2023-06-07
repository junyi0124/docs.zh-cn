---
title: CorMethodAttr 枚举
ms.date: 03/30/2017
api_name:
- CorMethodAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodAttr
helpviewer_keywords:
- CorMethodAttr enumeration [.NET Framework metadata]
ms.assetid: 4e0c3521-e54d-43c1-9857-cc76b49b8ffc
topic_type:
- apiref
ms.openlocfilehash: 6c3e721c24da217eaf2e8857377359e1c51b7b59
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677006"
---
# <a name="cormethodattr-enumeration"></a>CorMethodAttr 枚举

包含描述方法功能的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorMethodAttr {  
  
    mdMemberAccessMask          =   0x0007,  
    mdPrivateScope              =   0x0000,  
    mdPrivate                   =   0x0001,  
    mdFamANDAssem               =   0x0002,  
    mdAssem                     =   0x0003,  
    mdFamily                    =   0x0004,  
    mdFamORAssem                =   0x0005,  
    mdPublic                    =   0x0006,  
  
    mdStatic                    =   0x0010,  
    mdFinal                     =   0x0020,  
    mdVirtual                   =   0x0040,  
    mdHideBySig                 =   0x0080,  
  
    mdVtableLayoutMask          =   0x0100,  
    mdReuseSlot                 =   0x0000,  
    mdNewSlot                   =   0x0100,  
  
    mdCheckAccessOnOverride     =   0x0200,  
    mdAbstract                  =   0x0400,  
    mdSpecialName               =   0x0800,  
  
    mdPinvokeImpl               =   0x2000,  
    mdUnmanagedExport           =   0x0008,  
  
    mdReservedMask              =   0xd000,  
    mdRTSpecialName             =   0x1000,  
    mdHasSecurity               =   0x4000,  
    mdRequireSecObject          =   0x8000,  
  
} CorMethodAttr;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`mdMemberAccessMask`|指定成员访问。|  
|`mdPrivateScope`|指定不能引用成员。|  
|`mdPrivate`|指定只能由父类型访问成员。|  
|`mdFamANDAssem`|指定该成员只能由此程序集中的子类型访问。|  
|`mdAssem`|指定该成员由程序集中的任何人 accessibly。|  
|`mdFamily`|指定成员只能由类型和子类型访问。|  
|`mdFamORAssem`|指定成员可由派生类和其程序集中的其他类型访问。|  
|`mdPublic`|指定成员可由对范围具有访问权限的所有类型进行访问。|  
|`mdStatic`|指定将成员定义为类型的一部分而不是实例的成员。|  
|`mdFinal`|指定该方法不能被重写。|  
|`mdVirtual`|指定可以重写方法。|  
|`mdHideBySig`|指定该方法按名称和签名隐藏，而不是按名称隐藏。|  
|`mdVtableLayoutMask`|指定虚拟表布局。|  
|`mdReuseSlot`|指定重复使用用于虚拟表中此方法的槽。 这是默认设置。|  
|`mdNewSlot`|指定方法始终获取虚拟表中的新槽。|  
|`mdCheckAccessOnOverride`|指定方法可以由其可见的相同类型重写。|  
|`mdAbstract`|指定未实现该方法。|  
|`mdSpecialName`|指定该方法是特殊方法，其名称描述了操作方法。|  
|`mdPinvokeImpl`|指定使用 PInvoke 转发方法实现。|  
|`mdUnmanagedExport`|指定方法是导出到非托管代码的托管方法。|  
|`mdReservedMask`|保留供公共语言运行时内部使用。|  
|`mdRTSpecialName`|指定公共语言运行时应检查方法名称的编码。|  
|`mdHasSecurity`|指定该方法具有与之关联的安全性。|  
|`mdRequireSecObject`|指定该方法调用另一个包含安全代码的方法。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
