---
title: ICLRRuntimeInfo::BindAsLegacyV2Runtime 方法
ms.date: 03/30/2017
api_name:
- ICLRRuntimeInfo.BindAsLegacyV2Runtime
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRRuntimeInfo::BindAsLegacyV2Runtime
helpviewer_keywords:
- ICLRRuntimeInfo::BindAsLegacyV2Runtime method [.NET Framework hosting]
- BindAsLegacyV2Runtime method [.NET Framework hosting]
ms.assetid: 65fd55ac-4a24-4479-9384-a2e8013bfb2b
topic_type:
- apiref
ms.openlocfilehash: f0a03ecce49bbc3c1c03d037c9be31a8e994259d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732083"
---
# <a name="iclrruntimeinfobindaslegacyv2runtime-method"></a>ICLRRuntimeInfo::BindAsLegacyV2Runtime 方法

将所有旧公共语言运行时的当前运行时绑定 (CLR) 版本2激活策略决策。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT BindAsLegacyV2Runtime ();  
```  
  
## <a name="return-value"></a>返回值  

 此方法返回以下特定 Hresult：  
  
|HRESULT|说明|  
|-------------|-----------------|  
|S_OK|绑定成功，或已将此运行时绑定为旧版 CLR 版本2激活策略运行时。|  
|CLR_E_SHIM_LEGACYRUNTIMEALREADYBOUND|已将其他运行时绑定到旧版 CLR 版本2激活策略。|  
  
## <a name="remarks"></a>注解  

 如果当前运行时已绑定到所有旧版 CLR 版本2激活策略决策 (例如，通过使用 `useLegacyV2RuntimeActivationPolicy` 配置文件) 中[ \<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)上的属性，则此方法不会返回错误结果，而是 S_OK 结果，就像在方法成功绑定了旧激活策略时一样。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)
- [承载接口](hosting-interfaces.md)
- [承载](index.md)
- [\<startup> 元素](../../configure-apps/file-schema/startup/startup-element.md)
