---
title: ICorDebugController::HasQueuedCallbacks 方法
ms.date: 03/30/2017
api_name:
- ICorDebugController.HasQueuedCallbacks
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugController::HasQueuedCallbacks
helpviewer_keywords:
- HasQueuedCallbacks method [.NET Framework debugging]
- ICorDebugController::HasQueuedCallbacks method [.NET Framework debugging]
ms.assetid: 0d6a1cd9-370b-4462-adbf-e3980e897ea7
topic_type:
- apiref
ms.openlocfilehash: bd623f8bee2feafebe80c0c7513bcfb33d6ad367
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707888"
---
# <a name="icordebugcontrollerhasqueuedcallbacks-method"></a>ICorDebugController::HasQueuedCallbacks 方法

获取一个值，该值指示当前是否为指定线程排队任何托管回调。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT HasQueuedCallbacks (  
    [in] ICorDebugThread *pThread,  
    [out] BOOL           *pbQueued  
);  
```  
  
## <a name="parameters"></a>参数  

 `pThread`  
 中指向表示线程的 "ICorDebugThread" 对象的指针。  
  
 `pbQueued`  
 弄一个指向值的指针，该值为 `true` ; 如果当前为指定线程排队，则该值为; 否则为 `false` 。  
  
 如果为该参数指定了 null `pThread` ，则 `HasQueuedCallbacks` 将返回， `true` 如果当前有托管的回调排队等候任何线程。  
  
## <a name="remarks"></a>注解  

 每次都将调度一个回调，每次调用 [ICorDebugController：： Continue](icordebugcontroller-continue-method.md) 。 如果要报告同时发生的多个调试事件，则调试器可以检查此标志。  
  
 调试事件排队后，它们已发生，因此调试器必须释放整个队列，才能确保调试对象的状态。 `ICorDebugController::Continue`用于排出队列的 (调用 ) 。例如，如果队列在线程 *x* 上包含两个调试事件，并且调试器在第一个调试事件之后挂起线程 *x* ，然后调用 `ICorDebugController::Continue` ，则线程 *x* 的第二个调试事件会在线程已挂起的情况下进行调度。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
