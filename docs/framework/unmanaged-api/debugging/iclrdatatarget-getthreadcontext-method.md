---
title: ICLRDataTarget::GetThreadContext 方法
ms.date: 03/30/2017
api_name:
- ICLRDataTarget.GetThreadContext
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget::GetThreadContext
helpviewer_keywords:
- ICLRDataTarget::GetThreadContext method [.NET Framework debugging]
- GetThreadContext method, ICLRDataTarget interface [.NET Framework debugging]
ms.assetid: b9d8c3b5-3a2e-4225-95d4-dd052c4532c3
topic_type:
- apiref
ms.openlocfilehash: 35b7bff5d4d778a429ddc1dcd0206e6e8970ee4f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703494"
---
# <a name="iclrdatatargetgetthreadcontext-method"></a>ICLRDataTarget::GetThreadContext 方法

获取目标进程中给定线程的当前执行上下文。 此方法由公共语言运行时数据访问服务调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetThreadContext (  
    [in] ULONG32            threadID,  
    [in] ULONG32            contextFlags,  
    [in] ULONG32            contextSize,  
    [out, size_is(contextSize)]
        BYTE                *context  
);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 中目标进程中的线程的操作系统标识符。  
  
 `contextFlags`  
 中指定要返回的上下文部分的标志。 该实现将返回至少上下文的这些部分。  
  
 `contextSize`  
 中上下文的大小。  
  
 `context`  
 弄指向要在其中放置上下文的缓冲区的指针。  
  
 缓冲区中的数据 `context` 必须采用 Win32 结构的格式 `CONTEXT` 。 上下文指定处理器特定的寄存器数据，因此 Win32 结构的定义 `CONTEXT` 取决于处理器的体系结构。 有关 Win32 结构的定义，请参阅 WinNT. .h 头文件 `CONTEXT` 。  
  
## <a name="remarks"></a>注解  

 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
