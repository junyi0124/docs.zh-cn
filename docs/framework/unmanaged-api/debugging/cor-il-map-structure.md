---
title: COR_IL_MAP 结构
ms.date: 03/30/2017
api_name:
- COR_IL_MAP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_IL_MAP
helpviewer_keywords:
- COR_IL_MAP structure [.NET Framework debugging]
ms.assetid: 534ebc17-963d-4b26-8375-8cd940281db3
topic_type:
- apiref
ms.openlocfilehash: fb6b5d43e60b52c867535c42d59a098ef3c959bc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726377"
---
# <a name="cor_il_map-structure"></a>COR_IL_MAP 结构

指定函数的相对偏移量的更改。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _COR_IL_MAP {  
    ULONG32 oldOffset;
    ULONG32 newOffset;
    BOOL    fAccurate;  
} COR_IL_MAP;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`oldOffset`|旧的 Microsoft 中间语言 (MSIL) 相对于函数开头的偏移量。|  
|`newOffset`|相对于函数开头的新 MSIL 偏移量。|  
|`fAccurate`|`true` 如果已知正确的映射，则为;否则为 `false` 。|  
  
## <a name="remarks"></a>注解  

 映射的格式如下所示：调试器将假设，这 `oldOffset` 是在原始的未修改的 msil 代码内引用 msil 偏移量。 `newOffset`参数引用新的、经过检测的代码中的相应 MSIL 偏移量。  
  
 若要单步执行，应满足以下要求：  
  
- 地图应按升序排序。  
  
- 不应对已检测的 MSIL 代码进行重新排序。  
  
- 不应删除原始的 MSIL 代码。  
  
- 映射应包括用于将程序数据库中的所有序列点映射到 (PDB) 文件的项。  
  
 地图未插入缺失的条目。 下面的示例演示了一个映射及其结果。  
  
 Map:  
  
- 0个旧偏移，0个新偏移  
  
- 5旧偏移量，10个新偏移  
  
- 9旧偏移，20个新偏移  
  
 结果：  
  
- 早于0、1、2、3或4的偏移将映射到新的偏移量0。  
  
- 旧偏移量5、6、7或8将映射到新的偏移量10。  
  
- 旧偏移量9或更高将映射到新偏移量20。  
  
- 新偏移量为0、1、2、3、4、5、6、7、8或9，将映射到旧偏移量0。  
  
- 新偏移量为10、11、12、13、14、15、16、17、18或19，将映射到旧偏移量5。  
  
- 20或更高的新偏移量将映射到旧偏移量9。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cordebug.idl、Corprof.idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
