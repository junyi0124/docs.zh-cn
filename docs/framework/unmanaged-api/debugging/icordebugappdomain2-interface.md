---
title: ICorDebugAppDomain2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain2
helpviewer_keywords:
- ICorDebugAppDomain2 interface [.NET Framework debugging]
ms.assetid: 314d29f3-feb0-4a92-9530-b569c280cc31
topic_type:
- apiref
ms.openlocfilehash: f20ae6977504f958b7bfa8e2f073b7db6e8b822b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731470"
---
# <a name="icordebugappdomain2-interface"></a>ICorDebugAppDomain2 接口

提供使用数组、指针、函数指针和引用类型的方法。 此接口是 ICorDebugAppDomain 接口的扩展。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetArrayOrPointerType 方法](icordebugappdomain2-getarrayorpointertype-method.md)|获取指定类型的数组，或指定类型的指针或引用。|  
|[GetFunctionPointerType](icordebugappdomain2-getfunctionpointertype-method.md)|获取一个指针，该指针指向具有给定签名的函数。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
