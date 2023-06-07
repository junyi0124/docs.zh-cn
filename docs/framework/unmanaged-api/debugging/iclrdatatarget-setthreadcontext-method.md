---
title: ICLRDataTarget::SetThreadContext 方法
ms.date: 03/30/2017
api_name:
- ICLRDataTarget.SetThreadContext
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataTarget::SetThreadContext
helpviewer_keywords:
- SetThreadContext method, ICLRDataTarget interface [.NET Framework debugging]
- ICLRDataTarget::SetThreadContext method [.NET Framework debugging]
ms.assetid: 103c8502-81fe-40d7-9c1e-9008d8fb19e1
topic_type:
- apiref
ms.openlocfilehash: c135c051637858682c22db58d562e1d50eea562b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723696"
---
# <a name="iclrdatatargetsetthreadcontext-method"></a>ICLRDataTarget::SetThreadContext 方法

设置目标进程中指定线程的当前上下文。 此方法由公共语言运行时 (CLR) 数据访问服务调用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetThreadContext (  
    [in] ULONG32            threadID,  
    [in] ULONG32            contextSize,  
    [in, size_is(contextSize)]
         BYTE               *context  
);  
```  
  
## <a name="parameters"></a>参数  

 `threadID`  
 中目标进程中的线程的操作系统标识符。  
  
 `contextSize`  
 中上下文的大小。  
  
 `context`  
 中指向包含上下文的缓冲区的指针。  
  
 缓冲区中的数据 `context` 将采用 Win32 结构的格式 `CONTEXT` 。 上下文指定处理器特定的寄存器数据，因此 Win32 结构的定义 `CONTEXT` 取决于处理器的体系结构。 有关 Win32 结构的定义，请参阅 WinNT. .h 头文件 `CONTEXT` 。  
  
## <a name="remarks"></a>注解  

 此方法由调试应用程序的编写器实现。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataTarget 接口](iclrdatatarget-interface.md)
