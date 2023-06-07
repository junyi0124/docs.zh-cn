---
title: COR_PRF_TRANSITION_REASON 枚举
ms.date: 03/30/2017
api_name:
- COR_PRF_TRANSITION_REASON
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- COR_PRF_TRANSITION_REASON
helpviewer_keywords:
- COR_PRF_TRANSITION_REASON enumeration [.NET Framework profiling]
ms.assetid: da941118-01b7-4197-ae5b-9f2f8adcd623
topic_type:
- apiref
ms.openlocfilehash: 747313f217092652d5a9404fbf81383fa0828ee9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696656"
---
# <a name="cor_prf_transition_reason-enumeration"></a>COR_PRF_TRANSITION_REASON 枚举

指示从托管代码向非托管代码转换或从非托管代码向托管代码转换的原因。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    COR_PRF_TRANSITION_CALL,  
    COR_PRF_TRANSITION_RETURN  
} COR_PRF_TRANSITION_REASON;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`COR_PRF_TRANSITION_CALL`|由于调用了函数，因此转换。|  
|`COR_PRF_TRANSITION_RETURN`|由于从函数返回，因此转换。|  
  
## <a name="remarks"></a>注解  

 发生转换时，探查器会接收 [ICorProfilerCallback：： ManagedToUnmanagedTransition](icorprofilercallback-managedtounmanagedtransition-method.md) 或 [ICorProfilerCallback：： UnmanagedToManagedTransition](icorprofilercallback-unmanagedtomanagedtransition-method.md) 回调，其中任何一个都提供枚举的值， `COR_PRF_TRANSITION_REASON` 以指示转换的原因。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
