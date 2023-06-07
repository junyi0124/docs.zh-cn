---
title: METAHOST_CONFIG_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- METAHOST_CONFIG_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- METAHOST_CONFIG_FLAGS
helpviewer_keywords:
- METAHOST_CONFIG_FLAGS enumeration [.NET Framework hosting]
ms.assetid: 6f1e389f-ed99-4d6a-a0ba-72d7d869a01d
topic_type:
- apiref
ms.openlocfilehash: 01e55659bf2a348ec763f51112cbdcd706f27c84
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730001"
---
# <a name="metahost_config_flags-enumeration"></a>METAHOST_CONFIG_FLAGS 枚举

描述在 `pdwConfigFlags` [ICLRMetaHostPolicy：： GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md)方法的参数中返回的可能的标志，指示 `useLegacyV2RuntimeActivationPolicy` 配置文件的[ \<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)中的特性的状态和设置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_UNSET  = 0x00,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_TRUE   = 0x01,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_FALSE  = 0x02,  
    METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK   = 0x03  
} METAHOST_CONFIG_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_UNSET`|`useLegacyV2RuntimeActivationPolicy` [ \<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)中不存在该特性。|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_TRUE`|`useLegacyV2RuntimeActivationPolicy`特性存在并设置为 `true` 。|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_FALSE`|`useLegacyV2RuntimeActivationPolicy`特性存在并设置为 `false` 。|  
|`METAHOST_CONFIG_FLAGS_LEGACY_V2_ACTIVATION_POLICY_MASK`|将此掩码应用于在中返回的值，以 `pdwConfigFlags` 获取与相关的值 `useLegacyV2RuntimeActivationPolicy` 。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Metahost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
- [GetRequestedRuntime 方法](iclrmetahostpolicy-getrequestedruntime-method.md)
- [\<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)
