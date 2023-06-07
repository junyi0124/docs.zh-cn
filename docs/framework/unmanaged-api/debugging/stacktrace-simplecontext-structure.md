---
title: StackTrace_SimpleContext 结构
ms.date: 03/30/2017
api_name:
- StackTrace_SimpleContext
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- SimpleContext
helpviewer_keywords:
- SimpleContext structure [.NET Framework debugging]
- StackTrace_SimpleContext structure [.NET Framework debugging]
ms.assetid: d4cef11f-a8ca-49bc-a1b8-6631f9e28f3e
topic_type:
- apiref
ms.openlocfilehash: 30775b4a6f904d06b9c77e6b2b64aec693c446d7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671793"
---
# <a name="stacktrace_simplecontext-structure"></a>StackTrace_SimpleContext 结构

提供可用于代替完整的 `CONTEXT` 结构的简单上下文。  
  
## <a name="syntax"></a>语法  
  
```cpp  
struct StackTrace_SimpleContext  
{  
    ULONG64 StackOffset;       // ESP on x86  
    ULONG64 FrameOffset;       // EBP on x86  
    ULONG64 InstructionOffset; // EIP on x86  
};  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`StackOffset`|堆栈指针或输入堆栈指针 (ESP) 在 x86 平台上。|  
|`FrameOffset`|X86 平台上的帧偏移量或 EBP 寄存器。|  
|`InstructionOffset`|在 x86 平台上，指令指针或输入指令指针 (EIP) 。|  
  
## <a name="remarks"></a>注解  

 因为堆栈跟踪函数通常只需要返回地址、帧偏移量和堆栈地址，所以，您可以选择使用 `SimpleContext` 结构而不是大 `CONTEXT` 结构。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** SOS_Stacktrace。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
