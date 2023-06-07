---
title: ASM_CACHE_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- ASM_CACHE_FLAGS
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- ASM_CACHE_FLAGS
helpviewer_keywords:
- ASM_CACHE_FLAGS enumeration [.NET Framework fusion]
ms.assetid: 82e9a7da-321b-48b8-b239-52eaffda6be8
topic_type:
- apiref
ms.openlocfilehash: 6c6fab627f21977e85f9885ca4b49a0276faa5ce
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732159"
---
# <a name="asm_cache_flags-enumeration"></a>ASM_CACHE_FLAGS 枚举

指示由全局程序集缓存中的 [IAssemblyCacheItem](iassemblycacheitem-interface.md) 表示的程序集的源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    ASM_CACHE_ZAP       = 0x01,  
    ASM_CACHE_GAC       = 0x02,  
    ASM_CACHE_DOWNLOAD  = 0x04,  
    ASM_CACHE_ROOT      = 0x08,  
    ASM_CACHE_ROOT_EX   = 0x80  
} ASM_CACHE_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`ASM_CACHE_ZAP`|使用 Ngen.exe 枚举预编译的程序集的缓存。|  
|`ASM_CACHE_GAC`|枚举全局程序集缓存。|  
|`ASM_CACHE_DOWNLOAD`|枚举已按需下载或已进行卷影复制的程序集。|  
|`ASM_CACHE_ROOT`|指示 [GetCachePath](getcachepath-function.md) 函数应返回公共语言运行时 (CLR) 版本2.0 的全局程序集缓存的路径。 仅在对 [GetCachePath](getcachepath-function.md)的调用上下文中有意义。|  
|`ASM_CACHE_ROOT_EX`|指示 [GetCachePath](getcachepath-function.md) 函数应返回 CLR 版本4的全局程序集缓存的路径。 仅在对 [GetCachePath](getcachepath-function.md)的调用上下文中有意义。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetCachePath 函数](getcachepath-function.md)
- [IAssemblyCacheItem 接口](iassemblycacheitem-interface.md)
- [合成枚举](fusion-enumerations.md)
