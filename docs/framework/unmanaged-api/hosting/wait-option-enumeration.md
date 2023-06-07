---
title: WAIT_OPTION 枚举
ms.date: 03/30/2017
api_name:
- WAIT_OPTION
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- WAIT_OPTION
helpviewer_keywords:
- WAIT_OPTION enumeration [.NET Framework hosting]
ms.assetid: 962fc293-8ded-4b3b-90ce-2c21a4f1b244
topic_type:
- apiref
ms.openlocfilehash: 056d41df328de5796eec9f04589205d18b408f1f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732796"
---
# <a name="wait_option-enumeration"></a>WAIT_OPTION 枚举

包含一些值，这些值指示当公共语言运行时 (CLR) 块发出请求的操作时，主机应执行的操作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    WAIT_MSGPUMP       = 0x1,  
    WAIT_ALERTABLE     = 0x2,  
    WAIT_NOTINDEADLOCK = 0x4,  
} WAIT_OPTION;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`WAIT_ALERTABLE`|如果 CLR 调用 [IHostTask：： Alert](ihosttask-alert-method.md) 方法，则通知宿主应唤醒任务。|  
|`WAIT_MSGPUMP`|当线程被阻止时，通知宿主必须在当前 OS 线程上抽取消息。 运行时仅在线程上指定此值 <xref:System.Threading.ApartmentState.STA> 。|  
|`WAIT_NOTINDEADLOCK`|向宿主通知指定的同步请求无法被宿主中断。 也就是说，主机无法返回 `HOST_E_DEADLOCK` 。|  
  
## <a name="remarks"></a>注解  

 [IHostTaskManager：： Sleep](ihosttaskmanager-sleep-method.md)和[IHostTaskManager：： SwitchToTask](ihosttaskmanager-switchtotask-method.md)方法都采用此类型的参数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载枚举](hosting-enumerations.md)
