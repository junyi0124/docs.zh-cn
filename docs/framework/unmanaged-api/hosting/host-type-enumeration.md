---
title: HOST_TYPE 枚举
ms.date: 03/30/2017
api_name:
- HOST_TYPE
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- HOST_TYPE
helpviewer_keywords:
- HOST_TYPE enumeration [.NET Framework hosting]
ms.assetid: 51f848be-84c5-4036-9839-c762c576bbf5
topic_type:
- apiref
ms.openlocfilehash: 31640ada28af8e35554b91d5931d427fbaa8dcbe
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721850"
---
# <a name="host_type-enumeration"></a>HOST_TYPE 枚举

包含的值用于指定启动应用程序的主机的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    HOST_TYPE_DEFAULT     = 0x0,  
    HOST_TYPE_APPLAUNCH   = 0x1,  
    HOST_TYPE_CORFLAG     = 0x2  
} HOST_TYPE;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`HOST_TYPE_APPLAUNCH`|启动 AppLaunch.exe 的应用程序。<br /><br /> 将此值用于部分受信任的应用程序。|  
|`HOST_TYPE_CORFLAG`|直接启动该应用程序。 也就是说，从它自己的 .exe 文件启动该应用程序。<br /><br /> 将此值用于完全受信任的应用程序。|  
|`HOST_TYPE_DEFAULT`|与 HOST_TYPE_APPLAUNCH 相同。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
