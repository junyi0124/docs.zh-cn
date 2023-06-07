---
title: CorDebugRegister 枚举
ms.date: 03/30/2017
api_name:
- CorDebugRegister
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CorDebugRegister
helpviewer_keywords:
- CorDebugRegister enumeration [.NET Framework debugging]
ms.assetid: 003bb138-7960-4291-ac88-0d87e470ff70
topic_type:
- apiref
ms.openlocfilehash: 85df98e83396c9439c28dd41a3ffa02b820c9c3e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726049"
---
# <a name="cordebugregister-enumeration"></a>CorDebugRegister 枚举

指定与给定处理器体系结构关联的寄存器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugRegister {  
  
    REGISTER_INSTRUCTION_POINTER = 0,  
    REGISTER_STACK_POINTER,  
    REGISTER_FRAME_POINTER,  
  
    REGISTER_X86_EIP = 0,  
    REGISTER_X86_ESP,  
    REGISTER_X86_EBP,  
  
    REGISTER_X86_EAX,  
    REGISTER_X86_ECX,  
    REGISTER_X86_EDX,  
    REGISTER_X86_EBX,  
  
    REGISTER_X86_ESI,  
    REGISTER_X86_EDI,  
  
    REGISTER_X86_FPSTACK_0,  
    REGISTER_X86_FPSTACK_1,  
    REGISTER_X86_FPSTACK_2,  
    REGISTER_X86_FPSTACK_3,  
    REGISTER_X86_FPSTACK_4,  
    REGISTER_X86_FPSTACK_5,  
    REGISTER_X86_FPSTACK_6,  
    REGISTER_X86_FPSTACK_7,  
  
    REGISTER_AMD64_RIP = 0,  
    REGISTER_AMD64_RSP,  
    REGISTER_AMD64_RBP,  
    REGISTER_AMD64_RAX,  
    REGISTER_AMD64_RCX,  
    REGISTER_AMD64_RDX,  
    REGISTER_AMD64_RBX,  
    REGISTER_AMD64_RSI,  
    REGISTER_AMD64_RDI,  
    REGISTER_AMD64_R8,  
    REGISTER_AMD64_R9,  
    REGISTER_AMD64_R10,  
    REGISTER_AMD64_R11,  
    REGISTER_AMD64_R12,  
    REGISTER_AMD64_R13,  
    REGISTER_AMD64_R14,  
    REGISTER_AMD64_R15,  
  
    REGISTER_AMD64_XMM0,  
    REGISTER_AMD64_XMM1,  
    REGISTER_AMD64_XMM2,  
    REGISTER_AMD64_XMM3,  
    REGISTER_AMD64_XMM4,  
    REGISTER_AMD64_XMM5,  
    REGISTER_AMD64_XMM6,  
    REGISTER_AMD64_XMM7,  
    REGISTER_AMD64_XMM8,  
    REGISTER_AMD64_XMM9,  
    REGISTER_AMD64_XMM10,  
    REGISTER_AMD64_XMM11,  
    REGISTER_AMD64_XMM12,  
    REGISTER_AMD64_XMM13,  
    REGISTER_AMD64_XMM14,  
    REGISTER_AMD64_XMM15,  
  
    REGISTER_IA64_BSP = REGISTER_FRAME_POINTER,  
    REGISTER_IA64_R0  = REGISTER_IA64_BSP + 1,  
    REGISTER_IA64_F0  = REGISTER_IA64_R0  + 128,  
    REGISTER_ARM_PC = 0,  
    REGISTER_ARM_SP,  
    REGISTER_ARM_R0,  
    REGISTER_ARM_R1,  
    REGISTER_ARM_R2,  
    REGISTER_ARM_R3,  
    REGISTER_ARM_R4,  
    REGISTER_ARM_R5,  
    REGISTER_ARM_R6,  
    REGISTER_ARM_R7,  
    REGISTER_ARM_R8,  
    REGISTER_ARM_R9,  
    REGISTER_ARM_R10,  
    REGISTER_ARM_R11,  
    REGISTER_ARM_R12,  
    REGISTER_ARM_LR,  
  
} CorDebugRegister;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`REGISTER_INSTRUCTION_POINTER`|任何处理器上的指令指针寄存器。|  
|`REGISTER_STACK_POINTER`|任何处理器上的堆栈指针寄存器。|  
|`REGISTER_FRAME_POINTER`|任何处理器上的帧指针寄存器。|  
|`REGISTER_X86_EIP`|x86 处理器上的指令指针寄存器。|  
|`REGISTER_X86_ESP`|x86 处理器上的堆栈指针寄存器。|  
|`REGISTER_X86_EBP`|x86 处理器上的基指针寄存器。|  
|`REGISTER_X86_EAX`|x86 处理器上的 A 数据寄存器。|  
|`REGISTER_X86_ECX`|x86 处理器上的 C 数据寄存器。|  
|`REGISTER_X86_EDX`|x86 处理器上的 D 数据寄存器。|  
|`REGISTER_X86_EBX`|x86 处理器上的 B 数据寄存器。|  
|`REGISTER_X86_ESI`|x86 处理器上的源索引寄存器。|  
|`REGISTER_X86_EDI`|x86 处理器上的目标索引寄存器。|  
|`REGISTER_X86_FPSTACK_0`|x86 浮点 (FP) 处理器上的堆栈寄存器 0。|  
|`REGISTER_X86_FPSTACK_1`|x86 FP 处理器上的 #1 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_2`|x86 FP 处理器上的 #2 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_3`|x86 FP 处理器上的 #3 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_4`|x86 FP 处理器上的 #4 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_5`|x86 FP 处理器上的 #5 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_6`|x86 FP 处理器上的 #6 堆栈寄存器。|  
|`REGISTER_X86_FPSTACK_7`|x86 FP 处理器上的 #7 堆栈寄存器。|  
|`REGISTER_AMD64_RIP`|AMD64 处理器上的指令指针寄存器。|  
|`REGISTER_AMD64_RSP`|AMD64 处理器上的堆栈指针寄存器。|  
|`REGISTER_AMD64_RBP`|AMD64 处理器上的基指针寄存器。|  
|`REGISTER_AMD64_RAX`|AMD64 处理器上的 A 数据寄存器。|  
|`REGISTER_AMD64_RCX`|AMD64 处理器上的 C 数据寄存器。|  
|`REGISTER_AMD64_RDX`|AMD64 处理器上的 D 数据寄存器。|  
|`REGISTER_AMD64_RBX`|AMD64 处理器上的 B 数据寄存器。|  
|`REGISTER_AMD64_RSI`|AMD64 处理器上的源索引寄存器。|  
|`REGISTER_AMD64_RDI`|AMD64 处理器上的目标索引寄存器。|  
|`REGISTER_AMD64_R8`|AMD64 处理器上的 #8 数据寄存器。|  
|`REGISTER_AMD64_R9`|AMD64 处理器上的 #9 数据寄存器。|  
|`REGISTER_AMD64_R10`|AMD64 处理器上的 #10 数据寄存器。|  
|`REGISTER_AMD64_R11`|AMD64 处理器上的 #11 数据寄存器。|  
|`REGISTER_AMD64_R12`|AMD64 处理器上的 #12 数据寄存器。|  
|`REGISTER_AMD64_R13`|AMD64 处理器上的 #13 数据寄存器。|  
|`REGISTER_AMD64_R14`|AMD64 处理器上的 #14 数据寄存器。|  
|`REGISTER_AMD64_R15`|AMD64 处理器上的 #15 数据寄存器。|  
|`REGISTER_AMD64_XMM0`|AMD64 处理器上的 #0 多媒体寄存器。|  
|`REGISTER_AMD64_XMM1`|AMD64 处理器上的 #1 多媒体寄存器。|  
|`REGISTER_AMD64_XMM2`|AMD64 处理器上的 #2 多媒体寄存器。|  
|`REGISTER_AMD64_XMM3`|AMD64 处理器上的 #3 多媒体寄存器。|  
|`REGISTER_AMD64_XMM4`|AMD64 处理器上的 #4 多媒体寄存器。|  
|`REGISTER_AMD64_XMM5`|AMD64 处理器上的 #5 多媒体寄存器。|  
|`REGISTER_AMD64_XMM6`|AMD64 处理器上的 #6 多媒体寄存器。|  
|`REGISTER_AMD64_XMM7`|AMD64 处理器上的 #7 多媒体寄存器。|  
|`REGISTER_AMD64_XMM8`|AMD64 处理器上的 #8 多媒体寄存器。|  
|`REGISTER_AMD64_XMM9`|AMD64 处理器上的 #9 多媒体寄存器。|  
|`REGISTER_AMD64_XMM10`|AMD64 处理器上的 #10 多媒体寄存器。|  
|`REGISTER_AMD64_XMM11`|AMD64 处理器上的 #11 多媒体寄存器。|  
|`REGISTER_AMD64_XMM12`|AMD64 处理器上的 #12 多媒体寄存器。|  
|`REGISTER_AMD64_XMM13`|AMD64 处理器上的 #13 多媒体寄存器。|  
|`REGISTER_AMD64_XMM14`|AMD64 处理器上的 #14 多媒体寄存器。|  
|`REGISTER_AMD64_XMM15`|AMD64 处理器上的 #15 多媒体寄存器。|  
|`REGISTER_IA64_BSP`|IA-64 处理器上的堆栈指针寄存器。|  
|`REGISTER_IA64_R0`|IA-64 处理器上的 #0 数据寄存器。|  
|`REGISTER_IA64_F0`|IA-64 处理器上的 #0 FP 数据寄存器。|  
|`REGISTER_ARM_PC`|ARM 处理器上的程序计数器寄存器 (R15)。|  
|`REGISTER_ARM_SP`|ARM 处理器上的堆栈指针寄存器 (R13)。|  
|`REGISTER_ARM_R0`|ARM 处理器上的数据寄存器 R0。|  
|`REGISTER_ARM_R1`|ARM 处理器上的数据寄存器 R1。|  
|`REGISTER_ARM_R2`|ARM 处理器上的数据寄存器 R2。|  
|`REGISTER_ARM_R3`|ARM 处理器上的数据寄存器 R3。|  
|`REGISTER_ARM_R4`|ARM 处理器上的寄存器 R4。|  
|`REGISTER_ARM_R5`|ARM 处理器上的寄存器 R5。|  
|`REGISTER_ARM_R6`|ARM 处理器上的寄存器 R6。|  
|`REGISTER_ARM_R7`|ARM 处理器上的寄存器 R7（THUMB 帧指针）。|  
|`REGISTER_ARM_R8`|ARM 处理器上的寄存器 R8。|  
|`REGISTER_ARM_R9`|ARM 处理器上的寄存器 R9。|  
|`REGISTER_ARM_R10`|ARM 处理器上的寄存器 R10。|  
|`REGISTER_ARM_R11`|ARM 处理器上的帧指针。|  
|`REGISTER_ARM_R12`|ARM 处理器上的寄存器 R12。|  
|`REGISTER_ARM_LR`|ARM 处理器上的链接寄存器 (R14)。|  
  
## <a name="remarks"></a>注解  

 IA-64 处理器上共有 128 种通用数据寄存器和 128 种浮点数据寄存器，但只提供了值 `REGISTER_IA64_R0` 和 `REGISTER_IA64_F0`。 可按如下方式确定其他值：  
  
- 将寄存器号与 `REGISTER_IA64_R0` 相加，获得值 `REGISTER_IA64_R1` 到 `REGISTER_IA64_R127`（对应于 IA-64 处理器上的 #1 数据寄存器到 #127 数据寄存器）。  
  
- 将寄存器号与 `REGISTER_IA64_F0` 相加，获得值 `REGISTER_IA64_F1` 到 `REGISTER_IA64_F127`（对应于 IA-64 处理器上的 #1 FP 数据寄存器到 #127 FP 数据寄存器）。  
  
 例如，如果需要指定 IA-64 处理器上的 #83 数据寄存器，请使用 `REGISTER_IA64_R0` + 83。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
