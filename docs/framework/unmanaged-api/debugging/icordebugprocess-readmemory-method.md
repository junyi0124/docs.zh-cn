---
title: ICorDebugProcess::ReadMemory 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcess.ReadMemory
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess::ReadMemory
helpviewer_keywords:
- ReadMemory method [.NET Framework debugging]
- ICorDebugProcess::ReadMemory method [.NET Framework debugging]
ms.assetid: 28e4b2f6-9589-445c-be24-24a3306795e7
topic_type:
- apiref
ms.openlocfilehash: a0abc7168ff7bffdbb835c1c1bc93de9df6e381c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95694862"
---
# <a name="icordebugprocessreadmemory-method"></a>ICorDebugProcess::ReadMemory 方法

读取此进程的指定内存区域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReadMemory(  
    [in]  CORDB_ADDRESS address,
    [in]  DWORD size,  
    [out, size_is(size), length_is(size)] BYTE buffer[],  
    [out] SIZE_T *read);  
```  
  
## <a name="parameters"></a>参数  

 `address`  
 中一个 `CORDB_ADDRESS` 值，该值指定要读取的内存的基址。  
  
 `size`  
 中要从内存中读取的字节数。  
  
 `buffer`  
 弄接收内存内容的缓冲区。  
  
 `read`  
 弄一个指针，指向到指定缓冲区中传输的字节数。  
  
## <a name="remarks"></a>注解  

 此 `ReadMemory` 方法主要用于互操作调试，用于检查调试对象的非托管部分所使用的内存区域。 此方法还可用于读取 Microsoft 中间语言 (MSIL) 代码和本机 JIT 编译代码。  
  
 将从参数中返回的数据删除所有托管断点 `buffer` 。 不会对 [ICorDebugProcess2：： SetUnmanagedBreakpoint](icordebugprocess2-setunmanagedbreakpoint-method.md)设置的本机断点进行任何调整。  
  
 不会执行进程内存缓存。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
