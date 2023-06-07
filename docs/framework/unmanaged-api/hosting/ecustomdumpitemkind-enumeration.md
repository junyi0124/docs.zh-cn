---
title: ECustomDumpItemKind 枚举
ms.date: 03/30/2017
api_name:
- ECustomDumpItemKind
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ECustomDumpItemKind
helpviewer_keywords:
- ECustomDumpItemKind enumeration [.NET Framework hosting]
ms.assetid: 7105a6c8-6e4e-48de-ac3d-74ac75e5de2e
topic_type:
- apiref
ms.openlocfilehash: 6520815106ae1084d4281d5ebeb0680bb9bb91c5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686301"
---
# <a name="ecustomdumpitemkind-enumeration"></a>ECustomDumpItemKind 枚举

保留以供将来扩展 [CustomDumpItem](customdumpitem-structure.md) 结构。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    DUMP_ITEM_None = 0  
} ECustomDumpItemKind;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`DUMP_ITEM_None`|留待将来使用。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRErrorReportingManager 接口](iclrerrorreportingmanager-interface.md)
- [承载枚举](hosting-enumerations.md)
