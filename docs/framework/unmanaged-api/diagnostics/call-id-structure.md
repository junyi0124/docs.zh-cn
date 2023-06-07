---
title: CALL_ID 结构
ms.date: 03/30/2017
api_name:
- CALL_ID
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- CALL_ID
helpviewer_keywords:
- CALL_ID structure [.NET Framework debugging]
ms.assetid: bfd46324-afec-4782-9c18-586d81fb4740
topic_type:
- apiref
ms.openlocfilehash: 3f41dd969e25f7a42308ff0b7b2d617344284b38
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725243"
---
# <a name="call_id-structure"></a>CALL_ID 结构

向调试器提供有关正在调用的函数的信息。 有关详细信息，请参阅 [INotifySink2](inotifysink2-interface.md) 接口。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct tagCALL_ID  
{  
    LPCOLESTR       szMachine;  
    DWORD           dwPid;  
    USER_THREAD     *pUserThread;  
    STACK_ADDRESS   addrStackPointer;  
    LPCOLESTR       szEntryPoint;  
    LPCOLESTR       szDestinationMachine;  
} CALL_ID;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`szMachine`|标识正在进行调用的计算机。|  
|`dwPid`|标识计算机处理器。|  
|`pUserThread`|标识正在执行调用的线程。|  
|`addrStackPointer`|指定调用堆栈的地址。|  
|`szEntryPoint`|指定调用的地址。|  
|`szDestinationMachine`|标识要执行调用的计算机。|  
  
## <a name="requirements"></a>要求  

 **标头：** ProtocolNotify2 .idl  
  
## <a name="see-also"></a>另请参阅

- [INotifySink2 接口](inotifysink2-interface.md)
- [诊断符号存储区结构](diagnostics-symbol-store-structures.md)
