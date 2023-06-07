---
title: ICorDebugFunction2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugFunction2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFunction2
helpviewer_keywords:
- ICorDebugFunction2 interface [.NET Framework debugging]
ms.assetid: 2b936bef-9b75-48bf-859f-42e419c65f1c
topic_type:
- apiref
ms.openlocfilehash: d9e84a70d72db1338c80140ce3350774bfae4bca
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726270"
---
# <a name="icordebugfunction2-interface"></a>ICorDebugFunction2 接口

对 ICorDebugFunction 接口进行逻辑扩展，以便为跳过非用户代码仅我的代码逐步调试提供支持。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnumerateNativeCode 方法](icordebugfunction2-enumeratenativecode-method.md)| (尚未实现。 ) 获取一个指向 ICorDebugCodeEnum 的接口指针，该指针包含此 ICorDebugFunction2 对象引用的函数中的本机代码语句。|  
|[GetJMCStatus 方法](icordebugfunction2-getjmcstatus-method.md)|获取一个值，该值指示此函数是否被标记为 "用户代码"。|  
|[GetVersionNumber 方法](icordebugfunction2-getversionnumber-method.md)|获取此函数的 "编辑并继续" 版本。|  
|[SetJMCStatus 方法](icordebugfunction2-setjmcstatus-method.md)|将此函数标记仅我的代码单步执行。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
