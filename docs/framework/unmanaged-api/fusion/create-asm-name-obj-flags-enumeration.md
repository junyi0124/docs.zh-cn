---
title: CREATE_ASM_NAME_OBJ_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- CREATE_ASM_NAME_OBJ_FLAGS
api_location:
- fusion.dll
api_type:
- COM
f1_keywords:
- CREATE_ASM_NAME_OBJ_FLAGS
helpviewer_keywords:
- CREATE_ASM_NAME_OBJ_FLAGS enumeration [.NET Framework fusion]
ms.assetid: a5ed2fd0-c7d2-4603-aaca-5d0caad92675
topic_type:
- apiref
ms.openlocfilehash: 52d5ad3a18c102422e90621c7d1e23b2692c0000
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683222"
---
# <a name="create_asm_name_obj_flags-enumeration"></a>CREATE_ASM_NAME_OBJ_FLAGS 枚举

指定 [IAssemblyName 接口](iassemblyname-interface.md) 对象在由 [CreateAssemblyNameObject](createassemblynameobject-function.md) 函数构建时的特性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
  
    CANOF_PARSE_DISPLAY_NAME            = 0x1,  
    CANOF_SET_DEFAULT_VALUES            = 0x2,  
    CANOF_VERIFY_FRIEND_ASSEMBLYNAME    = 0x4,  
    CANOF_PARSE_FRIEND_DISPLAY_NAME     =
        CANOF_PARSE_DISPLAY_NAME | CANOF_VERIFY_FRIEND_ASSEMBLYNAME  
  
} CREATE_ASM_NAME_OBJ_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`CANOF_PARSE_DISPLAY_NAME`|指示传递的参数是文本标识。|  
|`CANOF_SET_DEFAULT_VALUES`|设置几个默认值。|  
|`CANOF_VERIFY_FRIEND_ASSEMBLYNAME`|验证友元程序集规则 (仅名称和公钥) 。 此成员仅供内部使用。|  
|`CANOF_PARSE_FRIEND_DISPLAY_NAME`|和标志的组合 `CANOF_PARSE_DISPLAY_NAME` `CANOF_VERIFY_FRIEND_ASSEMBLYNAME` 。 此成员仅供内部使用。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** 合成。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IAssemblyName 接口](iassemblyname-interface.md)
- [CreateAssemblyNameObject 函数](createassemblynameobject-function.md)
- [合成枚举](fusion-enumerations.md)
