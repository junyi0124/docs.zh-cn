---
title: ICorDebugMutableDataTarget 接口
ms.date: 03/30/2017
ms.assetid: 14aad5b3-84ab-4bbc-94e3-1eb92e258d10
ms.openlocfilehash: cd22707832504ca2f08299872bc39bca2af782bb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709344"
---
# <a name="icordebugmutabledatatarget-interface"></a>ICorDebugMutableDataTarget 接口

扩展 [ICorDebugDataTarget](icordebugdatatarget-interface.md) 接口以支持可变数据目标。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ContinueStatusChanged 方法](icordebugmutabledatatarget-continuestatuschanged-method.md)|更改指定线程上未完成的调试事件的延续状态。|  
|[SetThreadContext 方法](icordebugmutabledatatarget-setthreadcontext-method.md)|设置某个线程的上下文（寄存器值）。|  
|[WriteVirtual 方法](icordebugmutabledatatarget-writevirtual-method.md)|将内存写入目标进程地址空间。|  
  
## <a name="remarks"></a>注解  

 [ICorDebugDataTarget](icordebugdatatarget-interface.md)接口的这一扩展可通过调试工具来实现，这些工具要修改目标进程 (例如，执行实时侵害性调试) 。  
  
 如果不实现此接口，或者如果调用这些方法失败，则不会丢失任何基于检测的核心调试功能，从这个意义上来说，所有这些方法都是可选的。  这些方法中的任何故障 `HRESULT` 都将作为 ICorDebug 方法调用的 `HRESULT`传播。  
  
 请注意，一个 ICorDebug 方法调用可能会导致多个突变，并且没有任何机制可确保是否事务性应用了相关突变（全或无）。  这意味着如果某个突变在其他突变（对于相同的 ICorDebug 调用）成功后失败，则目标进程可能会处于不一致状态，且调试可能变得不可靠。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
