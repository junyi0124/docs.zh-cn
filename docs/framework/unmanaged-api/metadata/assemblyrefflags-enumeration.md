---
title: AssemblyRefFlags 枚举
ms.date: 03/30/2017
api_name:
- AssemblyRefFlags
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- AssemblyRefFlags
helpviewer_keywords:
- AssemblyRefFlags enumeration [.NET Framework metadata]
ms.assetid: decd4f46-f3b2-466f-9501-e74f2b86b846
topic_type:
- apiref
ms.openlocfilehash: 0a99d2f79645bdc46ff4db86d7280614eeb1faf5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732757"
---
# <a name="assemblyrefflags-enumeration"></a>AssemblyRefFlags 枚举

包含用于描述程序集引用的功能的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    arfFullOriginator = 0x0001  
} AssemblyRefFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`arfFullOriginator`|指定程序集引用包含有关程序集的发布服务器的完整的未哈哈希信息。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
- [IMetaDataAssemblyEmit 接口](imetadataassemblyemit-interface.md)
- [DefineAssemblyRef 方法](imetadataassemblyemit-defineassemblyref-method.md)
