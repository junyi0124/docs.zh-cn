---
title: COR_PRF_CLAUSE_TYPE 枚举
ms.date: 03/30/2017
api_name:
- COR_PRF_CLAUSE_TYPE
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_CLAUSE_TYPE
helpviewer_keywords:
- COR_PRF_CLAUSE_TYPE enumeration [.NET Framework profiling]
ms.assetid: f64c325a-ed3a-4aaf-b847-a88edbc4fefc
topic_type:
- apiref
ms.openlocfilehash: c3058229a3c2b3c529136dad70fea35a23708a33
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95718652"
---
# <a name="cor_prf_clause_type-enumeration"></a>COR_PRF_CLAUSE_TYPE 枚举

指示代码刚进入或离开的异常子句的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    COR_PRF_CLAUSE_NONE = 0,  
    COR_PRF_CLAUSE_FILTER = 1,  
    COR_PRF_CLAUSE_CATCH = 2,  
    COR_PRF_CLAUSE_FINALLY = 3,  
} COR_PRF_CLAUSE_TYPE;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`COR_PRF_CLAUSE_NONE`|异常子句无效。|  
|`COR_PRF_CLAUSE_FILTER`|Exception 子句是一个筛选器表达式。|  
|`COR_PRF_CLAUSE_CATCH`|Exception 子句为 `catch` 语句。|  
|`COR_PRF_CLAUSE_FINALLY`|Exception 子句为 `finally` 语句。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
