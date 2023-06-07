---
title: ICorDebugManagedCallback2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugManagedCallback2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugManagedCallback2
helpviewer_keywords:
- ICorDebugManagedCallback2 interface [.NET Framework debugging]
ms.assetid: cf7b7cfa-1c4b-4d8c-be70-4f9ed15a788b
topic_type:
- apiref
ms.openlocfilehash: 937a2fb322eb63461d90e215635e1b10ab6afd09
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689779"
---
# <a name="icordebugmanagedcallback2-interface"></a>ICorDebugManagedCallback2 接口

提供支持调试器异常处理和托管调试助手 (MDA) 的方法。 `ICorDebugManagedCallback2` 是 [ICorDebugManagedCallback](icordebugmanagedcallback-interface.md) 接口的逻辑扩展。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ChangeConnection 方法](icordebugmanagedcallback2-changeconnection-method.md)|通知调试器与指定连接关联的任务集已更改。|  
|[CreateConnection 方法](icordebugmanagedcallback2-createconnection-method.md)|通知调试器已创建新连接。|  
|[DestroyConnection 方法](icordebugmanagedcallback2-destroyconnection-method.md)|通知调试器指定的连接已终止。|  
|[Exception 方法](icordebugmanagedcallback2-exception-method.md)|通知调试器已开始搜索异常处理程序。|  
|[ExceptionUnwind 方法](icordebugmanagedcallback2-exceptionunwind-method.md)|在异常展开过程中提供状态通知。|  
|[FunctionRemapComplete 方法](icordebugmanagedcallback2-functionremapcomplete-method.md)|通知调试器，代码执行已切换到已编辑函数的新版本。|  
|[FunctionRemapOpportunity 方法](icordebugmanagedcallback2-functionremapopportunity-method.md)|通知调试程序代码执行已到达已编辑函数的较早版本中的序列点。|  
|[MDANotification 方法](icordebugmanagedcallback2-mdanotification-method.md)|提供通知，指出代码执行 (MDA) 消息中发生了托管调试助手。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugManagedCallback2`接口扩展 `ICorDebugManagedCallback` 接口以处理 .NET Framework 版本2.0 中引入的新调试事件。  
  
 调试程序在 `ICorDebugManagedCallback2` 调试 .NET Framework 2.0 应用程序时必须实现。 或的实例 `ICorDebugManagedCallback` `ICorDebugManagedCallback2` 作为回调对象传递给 [ICorDebug：： SetManagedHandler](icordebug-setmanagedhandler-method.md)。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [使用托管调试助手诊断错误](../../debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)
- [调试接口](debugging-interfaces.md)
- [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)
