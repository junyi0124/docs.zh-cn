---
title: CorDebugMDAFlags 枚举
ms.date: 03/30/2017
api_name:
- CorDebugMDAFlags
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugMDAFlags
helpviewer_keywords:
- CorDebugMDAFlags enumeration [.NET Framework debugging]
ms.assetid: 7c0c92fe-8bd2-477f-b307-aca0143732ca
topic_type:
- apiref
ms.openlocfilehash: 1bb99503481d917d41ae00a5ef73c8fa59e2a999
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696448"
---
# <a name="cordebugmdaflags-enumeration"></a>CorDebugMDAFlags 枚举

指定在其上激发托管调试助手 (MDA) 的线程的状态。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugMDAFlags {  
    MDA_FLAG_SLIP = 0x2  
} CorDebugMDAFlags;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`MDA_FLAG_SLIP`|触发 mda 后，触发 MDA 的线程发生了。|  
  
## <a name="remarks"></a>注解  

 当调用堆栈不再描述最初引发 MDA 的位置时，线程被视为已 *落后*。 这是一个不寻常的情况，导致在退出时线程执行无效操作的情况。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
