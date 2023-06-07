---
title: ESymbolReadingPolicy 枚举
ms.date: 03/30/2017
api_name:
- ESymbolReadingPolicy
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ESymbolReadingPolicy
helpviewer_keywords:
- ESymbolReadingPolicy enumeration [.NET Framework hosting]
ms.assetid: 4dc6c80d-b694-480b-a378-d5b18420ce17
topic_type:
- apiref
ms.openlocfilehash: 42ce1f02294db98c5c593a5f16de5226703d5f9d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733706"
---
# <a name="esymbolreadingpolicy-enumeration"></a>ESymbolReadingPolicy 枚举

包含一些值，这些值设置用于读取程序数据库 (PDB) 文件的策略。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    eSymbolReadingNever,  
    eSymbolReadingAlways,  
    eSymbolReadingFullTrustOnly  
} ESymbolReadingPolicy;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`eSymbolReadingAlways`|指定调试器应始终读取 PDB 文件。|  
|`eSymbolReadingFullTrustOnly`|指定调试器只应读取与完全信任程序集关联的 PDB 文件。|  
|`eSymbolReadingNever`|指定调试器不应读取 PDB 文件。|  
  
## <a name="remarks"></a>注解  

 `ESymbolReadingPolicy`枚举与[ICLRDebugManager：： SetSymbolReadingPolicy](iclrdebugmanager-setsymbolreadingpolicy-method.md)方法一起使用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
