---
title: CorAttributeTargets 枚举
ms.date: 03/30/2017
api_name:
- CorAttributeTargets
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorAttributeTargets
helpviewer_keywords:
- CorAttributeTargets enumeration [.NET Framework metadata]
ms.assetid: 694c0fa0-7011-41a9-9dfd-f0e16ea574b5
topic_type:
- apiref
ms.openlocfilehash: fbe721bad56ec2be434039f00e741ad9a177815f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718964"
---
# <a name="corattributetargets-enumeration"></a>CorAttributeTargets 枚举

指定可应用属性的应用程序元素。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorAttributeTargets  
{  
    catAssembly            = 0x0001,  
    catModule              = 0x0002,  
    catClass               = 0x0004,  
    catStruct              = 0x0008,  
    catEnum                = 0x0010,  
    catConstructor         = 0x0020,  
    catMethod              = 0x0040,  
    catProperty            = 0x0080,  
    catField               = 0x0100,  
    catEvent               = 0x0200,  
    catInterface           = 0x0400,  
    catParameter           = 0x0800,  
    catDelegate            = 0x1000,  
    catGenericParameter    = 0x4000,  
  
    catAll                 =
        catAssembly | catModule | catClass | catStruct |
        catEnum | catConstructor | catMethod | catProperty |
        catField | catEvent | catInterface | catParameter |
        catDelegate | catGenericParameter,  
  
    catClassMembers        =
        catClass | catStruct | catEnum | catConstructor |
        catMethod | catProperty | catField | catEvent |
        catDelegate | catInterface  
  
} CorAttributeTargets;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`catAssembly`|可以对程序集应用属性。|  
|`catModule`|特性可应用到可移植的可执行文件 ( .dll 或 .exe) 模块。|  
|`catClass`|可以对类应用属性。|  
|`catStruct`|可以对结构应用属性，即值类型。|  
|`catEnum`|可以对枚举应用属性。|  
|`catConstructor`|可以对构造函数应用属性。|  
|`catMethod`|可以对方法应用属性。|  
|`catProperty`|可以对属性 (Property) 应用属性 (Attribute)。|  
|`catField`|可以对字段应用属性。|  
|`catEvent`|可以对事件应用属性。|  
|`catInterface`|可以对接口应用属性。|  
|`catParameter`|可以对参数应用属性。|  
|`catDelegate`|可以对委托应用属性。|  
|`catGenericParameter`|可以对泛型参数应用属性。|  
|`catAll`|可以对任何应用程序元素应用属性。|  
|`catClassMembers`|特性可应用于类的成员。|  
  
## <a name="remarks"></a>注解  

 `CorAttributeTargets`枚举值可以与按位 "或" 运算组合在一起，以获取首选组合。  
  
 等效 `CorAttributeTargets` 托管 <xref:System.AttributeTargets?displayProperty=nameWithType> 枚举。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
