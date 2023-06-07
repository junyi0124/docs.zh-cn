---
title: EBindPolicyLevels 枚举
ms.date: 03/30/2017
api_name:
- EBindPolicyLevels
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- EBindPolicyLevels
helpviewer_keywords:
- EBindPolicyLevels enumeration [.NET Framework hosting]
ms.assetid: a9e00b4f-b6d0-4257-bd88-4fe9af97b8fa
topic_type:
- apiref
ms.openlocfilehash: a0992ca8ac4bfffef681c74de455a0eeb627a042
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726842"
---
# <a name="ebindpolicylevels-enumeration"></a>EBindPolicyLevels 枚举

提供用于指定应用或修改程序集策略的级别的标志。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    ePolicyLevelNone         = 0x0,  
    ePolicyLevelRetargetable = 0x1,  
    ePolicyUnifiedToCLR      = 0x2,  
    ePolicyLevelApp          = 0x4,  
    ePolicyLevelPublisher    = 0x8,  
    ePolicyLevelHost         = 0x10,  
    ePolicyLevelAdmin        = 0x20  
    ePolicyPortability       = 0x40  
} EBindPolicyLevels;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`ePolicyLevelAdmin`|指定应在管理员级别应用策略。|  
|`ePolicyLevelApp`|指定应在应用程序级别应用策略。|  
|`ePolicyLevelHost`|指定应在主机级别上应用策略。|  
|`ePolicyLevelNone`|不指定策略级别标志。|  
|`ePolicyLevelPublisher`|指定应在发布服务器级别应用策略。|  
|`ePolicyLevelRetargetable`|指定策略应在可变级别上适用。|  
|`ePolicyPortability`|指定策略应支持 .NET Framework 程序集的实现之间的可移植性。 请参阅 [\<supportPortability>](../../configure-apps/file-schema/runtime/supportportability-element.md) 配置文件元素。|  
|`ePolicyUnifiedToCLR`|指定应将策略统一到公共语言运行时的 (CLR) 。|  
  
## <a name="remarks"></a>注解  

 此枚举将传递给 [ICLRHostBindingPolicyManager](iclrhostbindingpolicymanager-interface.md) 接口的方法，以指定应用程序策略中的更改。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [承载枚举](hosting-enumerations.md)
