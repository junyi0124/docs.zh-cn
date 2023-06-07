---
title: ICorDebug::DebugActiveProcess 方法
ms.date: 03/30/2017
api_name:
- ICorDebug.DebugActiveProcess
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebug::DebugActiveProcess
helpviewer_keywords:
- DebugActiveProcess method [.NET Framework debugging]
- ICorDebug::DebugActiveProcess method [.NET Framework debugging]
ms.assetid: fdab0ade-7f56-4fa2-b3ef-f7a1d2852bba
topic_type:
- apiref
ms.openlocfilehash: 1713623fa575bea6df649106b37212f7aeaee6db
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723462"
---
# <a name="icordebugdebugactiveprocess-method"></a>ICorDebug::DebugActiveProcess 方法

将调试器附加到现有进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DebugActiveProcess (  
    [in]  DWORD               id,  
    [in]  BOOL                win32Attach,  
    [out] ICorDebugProcess    **ppProcess  
);  
```  
  
## <a name="parameters"></a>参数  

 `id`  
 中调试器要附加到的进程的 ID。  
  
 `win32Attach`  
 中 `true` 如果调试器应该表现为进程的 Win32 调试器并调度非托管回调，则设置为的布尔值; 否则为 `false` 。  
  
 `ppProcess`  
 弄一个指向 "ICorDebugProcess" 对象地址的指针，该对象表示调试器已附加到的进程。  
  
## <a name="remarks"></a>注解  

 Win9x 和非 x86 平台都不支持互操作调试，如基于 IA-64 和基于 AMD64 的平台。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebug 接口](icordebug-interface.md)
