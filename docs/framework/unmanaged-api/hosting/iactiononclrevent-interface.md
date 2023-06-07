---
title: IActionOnCLREvent 接口
ms.date: 03/30/2017
api_name:
- IActionOnCLREvent
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IActionOnCLREvent
helpviewer_keywords:
- IActionOnCLREvent interface [.NET Framework hosting]
ms.assetid: b5f9b41e-7301-429c-911f-21d5422292b3
topic_type:
- apiref
ms.openlocfilehash: 8ca4bb1fe35451f95f752a4e71f5f0b541b55e58
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95721772"
---
# <a name="iactiononclrevent-interface"></a>IActionOnCLREvent 接口

提供了 [IActionOnCLREvent：： OnEvent](iactiononclrevent-onevent-method.md) 方法，该方法对已通过调用 [ICLROnEventManager：： RegisterActionOnEvent](iclroneventmanager-registeractiononevent-method.md) 方法注册的事件执行回调。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[OnEvent 方法](iactiononclrevent-onevent-method.md)|对已注册的事件执行回调。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [EClrEvent 枚举](eclrevent-enumeration.md)
- [ICLRControl 接口](iclrcontrol-interface.md)
- [ICLROnEventManager 接口](iclroneventmanager-interface.md)
- [承载接口](hosting-interfaces.md)
