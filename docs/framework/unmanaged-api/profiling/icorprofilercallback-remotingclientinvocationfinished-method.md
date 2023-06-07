---
title: ICorProfilerCallback::RemotingClientInvocationFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.RemotingClientInvocationFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::RemotingClientInvocationFinished
helpviewer_keywords:
- RemotingClientInvocationFinished method [.NET Framework profiling]
- ICorProfilerCallback::RemotingClientInvocationFinished method [.NET Framework profiling]
ms.assetid: ea4b283b-1210-4f41-a7a2-c398b1adde4e
topic_type:
- apiref
ms.openlocfilehash: d41ccd30707eba269bbac7231e06792363615544
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719315"
---
# <a name="icorprofilercallbackremotingclientinvocationfinished-method"></a>ICorProfilerCallback::RemotingClientInvocationFinished 方法

通知探查器远程处理调用已在客户端上完成运行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT RemotingClientInvocationFinished();  
```  
  
## <a name="remarks"></a>备注  

 如果远程处理调用是同步的，则它还会在服务器上运行到完成。 如果远程处理调用是异步的，则在处理调用时可能仍会出现回复。 如果需要答复，则会作为对 [ICorProfilerCallback：： RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md) 的调用，以及对的其他调用 `RemotingClientInvocationFinished` 以指示异步调用所需的辅助处理。  
  
 以下每对回调都将在同一线程上进行：  
  
- `RemotingClientInvocationStarted` 和 [ICorProfilerCallback：： RemotingClientSendingMessage](icorprofilercallback-remotingclientsendingmessage-method.md)  
  
- [ICorProfilerCallback：： RemotingClientReceivingReply](icorprofilercallback-remotingclientreceivingreply-method.md) 和 [ICorProfilerCallback：： RemotingClientInvocationFinished](icorprofilercallback-remotingclientinvocationfinished-method.md)  
  
- [ICorProfilerCallback：： RemotingServerInvocationReturned](icorprofilercallback-remotingserverinvocationreturned-method.md) 和 [ICorProfilerCallback：： RemotingServerSendingReply](icorprofilercallback-remotingserversendingreply-method.md)  
  
 应注意远程处理回调的以下问题：  
  
- 远程处理函数的执行不会由探查器 API 反射，因此无法正确地接收从客户端调用并在服务器上执行的函数的通知。 实际调用是通过代理对象进行的;对于探查器，似乎某些函数是 JIT 编译的，但从未使用过。  
  
- 探查器不会为异步远程处理事件接收准确的通知。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
