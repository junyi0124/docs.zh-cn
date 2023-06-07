---
title: IGCThreadControl 接口
ms.date: 03/30/2017
api_name:
- IGCThreadControl
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IGCThreadControl
helpviewer_keywords:
- IGCThreadControl interface [.NET Framework hosting]
ms.assetid: 3ff04d75-85ac-4df9-886d-dbaa037c0552
topic_type:
- apiref
ms.openlocfilehash: 02facbb0ff1c0f8978d4f4f720ab370f70f07fe2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721681"
---
# <a name="igcthreadcontrol-interface"></a>IGCThreadControl 接口

提供一些方法，这些方法用于参与要阻止垃圾回收的线程的计划。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[SuspensionEnding 方法](igcthreadcontrol-suspensionending-method.md)|向宿主通知运行时在垃圾回收或其他挂起后正在恢复线程。|  
|[SuspensionStarting 方法](igcthreadcontrol-suspensionstarting-method.md)|通知宿主运行时正在为垃圾回收或其他挂起开始线程挂起。|  
|[ThreadIsBlockingForSuspension 方法](igcthreadcontrol-threadisblockingforsuspension-method.md)|通知宿主发出调用的线程将要阻止，可能用于垃圾回收或其他挂起。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载接口](hosting-interfaces.md)
