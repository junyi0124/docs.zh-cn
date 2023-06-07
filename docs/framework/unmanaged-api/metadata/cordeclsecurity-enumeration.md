---
title: CorDeclSecurity 枚举
ms.date: 03/30/2017
api_name:
- CorDeclSecurity
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorDeclSecurity
helpviewer_keywords:
- CorDeclSecurity enumeration [.NET Framework metadata]
ms.assetid: 864f1267-d267-4696-8df7-1f83f8444d6f
topic_type:
- apiref
ms.openlocfilehash: dd599ce8c63fa94e1a18b4e2d18fa334238728bd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718873"
---
# <a name="cordeclsecurity-enumeration"></a>CorDeclSecurity 枚举

指定可以使用声明性安全执行的安全操作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDeclSecurity {  
  
    dclActionMask               =   0x001f,  
    dclActionNil                =   0x0000,  
    dclRequest                  =   0x0001,  
    dclDemand                   =   0x0002,  
    dclAssert                   =   0x0003,  
    dclDeny                     =   0x0004,  
    dclPermitOnly               =   0x0005,  
    dclLinktimeCheck            =   0x0006,  
    dclInheritanceCheck         =   0x0007,  
    dclRequestMinimum           =   0x0008,  
    dclRequestOptional          =   0x0009,  
    dclRequestRefuse            =   0x000a,  
    dclPrejitGrant              =   0x000b,  
    dclPrejitDenied             =   0x000c,  
    dclNonCasDemand             =   0x000d,  
    dclNonCasLinkDemand         =   0x000e,  
    dclNonCasInheritance        =   0x000f,  
    dclLinkDemandChoice         =   0x0010,  
    dclInheritanceDemandChoice  =   0x0011,  
    dclDemandChoice             =   0x0012,  
    dclMaximumValue             =   0x0012  
  
} CorDeclSecurity;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`dclActionMask`|保留。|  
|`dclActionNil`|保留。|  
|`dclRequest`|保留。|  
|`dclDemand`|要求调用堆栈中的所有高级调用方已被授予当前权限对象所指定的权限。|  
|`dclAssert`|调用代码可以访问当前权限对象所标识的资源，即使堆栈中的高级调用方未被授予访问该资源的权限|  
|`dclDeny`|即使调用方已被授予访问当前权限对象所指定的资源的权限，也会拒绝这些调用方访问该资源。|  
|`dclPermitOnly`|仅可以访问此权限对象所指定的资源，即使代码已被授予访问其他资源的权限。|  
|`dclLinktimeCheck`|需要在给定的时间段内向直接调用方授予指定的权限。|  
|`dclInheritanceCheck`|需要继承另一个类或重写方法的派生类已被授予指定的权限。|  
|`dclRequestMinimum`|调用方可以请求运行代码所需的最小权限。 此操作仅可以在程序集的作用域内使用。|  
|`dclRequestOptional`|调用方可以请求其他可选权限，这 (不需要运行) 。 此请求隐式拒绝所有未明确请求的其他权限。 此操作仅可以在程序集的作用域内使用。|  
|`dclRequestRefuse`|调用方对可能被误用的权限的请求将不会被授予。 此操作仅可以在程序集的作用域内使用。|  
|`dclPrejitGrant`|保留。|  
|`dclPrejitDenied`|保留。|  
|`dclNonCasDemand`|保留。|  
|`dclNonCasLinkDemand`|要求直接调用方已被授予指定的权限。|  
|`dclNonCasInheritance`|保留。|  
|`dclLinkDemandChoice`|保留。|  
|`dclInheritanceDemandChoice`|保留。|  
|`dclDemandChoice`|保留。|  
|`dclMaximumValue`|保留。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
