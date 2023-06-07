---
title: COR_ACTIVE_FUNCTION 结构
ms.date: 03/30/2017
api_name:
- COR_ACTIVE_FUNCTION
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_ACTIVE_FUNCTION
helpviewer_keywords:
- COR_ACTIVE_FUNCTION structure [.NET Framework debugging]
ms.assetid: ed86185f-2152-459c-961f-10c06d62e83f
topic_type:
- apiref
ms.openlocfilehash: c50ba530d78296ebb956329b2f34b4f1e5cae94c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727414"
---
# <a name="cor_active_function-structure"></a>COR_ACTIVE_FUNCTION 结构

包含有关在线程框架中当前处于活动状态的函数的信息。 此结构由 [ICorDebugThread2：： GetActiveFunctions](icordebugthread2-getactivefunctions-method.md) 方法使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct  _COR_ACTIVE_FUNCTION {  
    ICorDebugAppDomain   *pAppDomain;  
    ICorDebugModule      *pModule;  
    ICorDebugFunction2   *pFunction;  
    ULONG32              ilOffset;  
    ULONG32              flags;  
} COR_ACTIVE_FUNCTION;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`pAppDomain`|指向字段的应用程序域所有者的指针 `ilOffset` 。|  
|`pModule`|指向字段的模块所有者的指针 `ilOffset` 。|  
|`pFunction`|指向字段的函数所有者的指针 `ilOffset` 。|  
|`ilOffset`|Microsoft 中间语言 (MSIL) 帧偏移量。|  
|`flags`|保留以供将来进行扩展。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cordebug.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
