---
title: ICorDebugProcess::WriteMemory 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.WriteMemory
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::WriteMemory
helpviewer_keywords:
- ICorDebugProcess::WriteMemory method [.NET Framework debugging]
- WriteMemory method [.NET Framework debugging]
ms.assetid: d5c07d86-045d-4391-893b-0bcd2959f90e
topic_type:
- apiref
ms.openlocfilehash: 18416954517c3cac09d013b8075bd097305a1dca
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673951"
---
# <a name="icordebugprocesswritememory-method"></a>ICorDebugProcess::WriteMemory 方法

将数据写入到此进程中的内存区域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT WriteMemory(  
    [in]  CORDB_ADDRESS address,  
    [in]  DWORD size,  
    [in, size_is(size)] BYTE buffer[],  
    [out] SIZE_T *written);  
```  
  
## <a name="parameters"></a>参数  

 `address`  
 中一个 `CORDB_ADDRESS` 值，该值是写入数据的内存区域的基址。 在进行数据传输之前，系统将验证指定大小（从基址开始）的内存区域是否可供写入。 如果不可访问，则该方法将失败。  
  
 `size`  
 中要写入内存区域的字节数。  
  
 `buffer`  
 中包含要写入的数据的缓冲区。  
  
 `written`  
 弄指向一个变量的指针，该变量接收写入到此进程中的内存区域的字节数。 如果 `written` 为 NULL，则忽略此参数。  
  
## <a name="remarks"></a>注解  

 数据将在任何断点后面自动写入。 在 .NET Framework 版本2.0 中，本机调试器不应使用此方法将断点注入到指令流中。 改 [为使用 ICorDebugProcess2：： SetUnmanagedBreakpoint](icordebugprocess2-setunmanagedbreakpoint-method.md) 。  
  
 `WriteMemory`方法只应在托管代码之外使用。 如果使用不当，此方法可能会损坏运行时。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
