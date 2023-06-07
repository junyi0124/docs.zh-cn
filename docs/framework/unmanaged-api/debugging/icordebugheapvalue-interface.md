---
title: ICorDebugHeapValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugHeapValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugHeapValue
helpviewer_keywords:
- ICorDebugHeapValue interface [.NET Framework debugging]
ms.assetid: 1bca66db-0359-4ae8-846e-e35f7e547e8b
topic_type:
- apiref
ms.openlocfilehash: ee3ea319360bba1a113c15daf8cf143ea512e5cd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733316"
---
# <a name="icordebugheapvalue-interface"></a>ICorDebugHeapValue 接口

"ICorDebugValue" 的子类，它表示公共语言运行时 (CLR) 垃圾回收器收集的对象。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateRelocBreakpoint 方法](icordebugheapvalue-createrelocbreakpoint-method.md)|未实现。|  
|[IsValid 方法](icordebugheapvalue-isvalid-method.md)|获取一个值，该值指示由此表示的对象是否 `ICorDebugHeapValue` 有效，或者是否已被垃圾回收器回收。 此方法在 .NET Framework 版本2.0 中已弃用。|  
  
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
