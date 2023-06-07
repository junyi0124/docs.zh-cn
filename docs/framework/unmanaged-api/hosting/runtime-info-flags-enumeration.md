---
title: RUNTIME_INFO_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- RUNTIME_INFO_FLAGS
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- RUNTIME_INFO_FLAGS
helpviewer_keywords:
- RUNTIME_INFO_FLAGS enumeration [.NET Framework hosting]
ms.assetid: adba37be-f775-4cdb-8919-5746ce694f33
topic_type:
- apiref
ms.openlocfilehash: 6f4fbb40053628d60ba7f094fcb5d50a94d63e1a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729936"
---
# <a name="runtime_info_flags-enumeration"></a>RUNTIME_INFO_FLAGS 枚举

包含一些值，这些值指示应返回 (CLR) 的公共语言运行时信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
  
    RUNTIME_INFO_UPGRADE_VERSION             = 0x01,  
    RUNTIME_INFO_REQUEST_IA64                = 0x02,  
    RUNTIME_INFO_REQUEST_AMD64               = 0x04,  
    RUNTIME_INFO_REQUEST_X86                 = 0x08,  
    RUNTIME_INFO_DONT_RETURN_DIRECTORY       = 0x10,  
    RUNTIME_INFO_DONT_RETURN_VERSION         = 0x20,  
    RUNTIME_INFO_DONT_SHOW_ERROR_DIALOG      = 0x40,  
    RUNTIME_INFO_IGNORE_ERROR_MODE           = 0x1000  
  
} RUNTIME_INFO_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`RUNTIME_INFO_DONT_RETURN_DIRECTORY`|指示不应包含目录信息。|  
|`RUNTIME_INFO_DONT_RETURN_VERSION`|指示不应包含版本信息。|  
|`RUNTIME_INFO_DONT_SHOW_ERROR_DIALOG`|指示在失败时不应显示错误对话框。|  
|`RUNTIME_INFO_IGNORE_ERROR_MODE`|指示应重写调用带有 SEM_FAILCRITICALERRORS 标志的 [SetErrorMode](/windows/win32/api/errhandlingapi/nf-errhandlingapi-seterrormode) 函数的效果。 也就是说，出现故障时应显示安装对话框，而不是取消显示。|  
|`RUNTIME_INFO_REQUEST_AMD64`|指示对与 AMD-64 兼容的运行时版本的信息的请求。|  
|`RUNTIME_INFO_REQUEST_IA64`|指示对与 IA-64 兼容的运行时版本有关的信息的请求。|  
|`RUNTIME_INFO_REQUEST_X86`|指示对与 x86 兼容的运行时版本有关的信息的请求。|  
|`RUNTIME_INFO_UPGRADE_VERSION`|指示应包含版本升级信息。|  
  
## <a name="remarks"></a>注解  

 以下平台体系结构标志一次只能指定一个，且不能组合使用：  
  
- RUNTIME_INFO_REQUEST_IA64  
  
- RUNTIME_INFO_REQUEST_AMD64  
  
- RUNTIME_INFO_REQUEST_X86  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
