---
title: CorCallingConvention 枚举
ms.date: 03/30/2017
api_name:
- CorCallingConvention
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorCallingConvention
helpviewer_keywords:
- CorCallingConvention enumeration [.NET Framework metadata]
ms.assetid: 69156fbf-7219-43bf-b4b8-b13f1a2fcb86
topic_type:
- apiref
ms.openlocfilehash: c9b20500a4a9e4649a938e00e3b059d1395da1d3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718925"
---
# <a name="corcallingconvention-enumeration"></a>CorCallingConvention 枚举

包含一些值，用于描述托管代码中执行的调用约定类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorCallingConvention  
{  
    IMAGE_CEE_CS_CALLCONV_DEFAULT       = 0x0,  
  
    IMAGE_CEE_CS_CALLCONV_VARARG        = 0x5,  
    IMAGE_CEE_CS_CALLCONV_FIELD         = 0x6,  
    IMAGE_CEE_CS_CALLCONV_LOCAL_SIG     = 0x7,  
    IMAGE_CEE_CS_CALLCONV_PROPERTY      = 0x8,  
    IMAGE_CEE_CS_CALLCONV_UNMGD         = 0x9,  
    IMAGE_CEE_CS_CALLCONV_GENERICINST   = 0xa,  
    IMAGE_CEE_CS_CALLCONV_NATIVEVARARG  = 0xb,  
    IMAGE_CEE_CS_CALLCONV_MAX           = 0xc,  
  
    IMAGE_CEE_CS_CALLCONV_MASK          = 0x0f,  
    IMAGE_CEE_CS_CALLCONV_HASTHIS       = 0x20,  
    IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS  = 0x40,  
    IMAGE_CEE_CS_CALLCONV_GENERIC       = 0x10  
  
} CorCallingConvention;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`IMAGE_CEE_CS_CALLCONV_DEFAULT`|指示默认调用约定。|  
|`IMAGE_CEE_CS_CALLCONV_VARARG`|指示该方法采用可变数量的参数。|  
|`IMAGE_CEE_CS_CALLCONV_FIELD`|指示调用为字段。|  
|`IMAGE_CEE_CS_CALLCONV_LOCAL_SIG`|指示调用为本地方法。|  
|`IMAGE_CEE_CS_CALLCONV_PROPERTY`|指示对属性的调用。|  
|`IMAGE_CEE_CS_CALLCONV_UNMGD`|指示调用是非托管的。|  
|`IMAGE_CEE_CS_CALLCONV_GENERICINST`|指示泛型方法的实例化。|  
|`IMAGE_CEE_CS_CALLCONV_NATIVEVARARG`|指示对采用可变数目的参数的方法进行64位 PInvoke 调用。|  
|`IMAGE_CEE_CS_CALLCONV_MAX`|描述无效的4位值。|  
|`IMAGE_CEE_CS_CALLCONV_MASK`|指示调用约定由四位底部描述。|  
|`IMAGE_CEE_CS_CALLCONV_HASTHIS`|指示上位描述了一个 `this` 参数。|  
|`IMAGE_CEE_CS_CALLCONV_EXPLICITTHIS`|指示 `this` 签名中显式描述了一个参数。|  
|`IMAGE_CEE_CS_CALLCONV_GENERIC`|指示包含类型参数的显式数目的泛型方法签名。 这优先于普通参数计数。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
