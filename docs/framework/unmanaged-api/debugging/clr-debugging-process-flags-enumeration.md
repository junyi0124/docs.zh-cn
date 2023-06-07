---
title: CLR_DEBUGGING_PROCESS_FLAGS 枚举
ms.date: 03/30/2017
api_name:
- CLR_DEBUGGING_PROCESS_FLAGS
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CLR_DEBUGGING_PROCESS_FLAG
helpviewer_keywords:
- CLR_DEBUGGING_PROCESS_FLAGS enumeration [.NET Framework debugging]
ms.assetid: 85b85fde-1f87-490b-ba8d-d604670959c3
topic_type:
- apiref
ms.openlocfilehash: dd148d23d6e29f03052d3bbf1fcd5d02fb332a0a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729845"
---
# <a name="clr_debugging_process_flags-enumeration"></a>CLR_DEBUGGING_PROCESS_FLAGS 枚举

提供 [ICLRDebugging：： OpenVirtualProcess](iclrdebugging-openvirtualprocess-method.md) 方法使用的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CLR_DEBUGGING_PROCESS_FLAGS  
{  
   CLR_DEBUGGING_MANAGED_EVENT_PENDING = 1,  
   CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH = 2  
}  CLR_DEBUGGING_PROCESS_FLAGS;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`CLR_DEBUGGING_MANAGED_EVENT_PENDING`|此运行时有一个要发送的非追赶托管调试器事件。 请参阅 "备注" 部分，了解追赶事件与非追赶事件之间的区别。|  
|`CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH`|挂起的托管事件是 <xref:System.Diagnostics.Debugger.Launch%2A?displayProperty=nameWithType> 请求。|  
  
## <a name="remarks"></a>注解  

 追赶事件包括进程、应用程序域、程序集、模块和线程创建通知，这会使调试器在附加到进程后进入当前状态。 标记指示的非追赶事件 `CLR_DEBUGGING_MANAGED_EVENT_PENDING` 包含所有其他调试器事件，例如异常和托管调试助手 (MDA) 通知。  
  
 该 `CLR_DEBUGGING_MANAGED_EVENT_DEBUGGER_LAUNCH` 标志使运行时能够区分终止异常和请求以附加可取消的托管调试器。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Metahost，Metahost  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
- [调试](index.md)
