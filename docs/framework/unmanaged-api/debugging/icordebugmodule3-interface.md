---
title: ICorDebugModule3 接口
ms.date: 03/30/2017
api_name:
- ICorDebugModule3
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule3
helpviewer_keywords:
- ICorDebugModule3 interface [.NET Framework debugging]
ms.assetid: 0b69f945-263a-4e11-8512-89d27f6ea296
topic_type:
- apiref
ms.openlocfilehash: 543a1a3c79b6cf3eb799da5844f35286dfa91940
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95709552"
---
# <a name="icordebugmodule3-interface"></a>ICorDebugModule3 接口

为动态模块创建符号读取器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
interface ICorDebugModule3 : IUnknown  
{  
    HRESULT CreateReaderForInMemorySymbols  
      (  
      [in] REFIID riid,  
      [out][iid_is(riid)] void **  ppObj  
      );  
};  
```  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ICorDebugModule3::CreateReaderForInMemorySymbols 方法](icordebugmodule3-createreaderforinmemorysymbols-method.md)| (通常为动态模块的 [ISymUnmanagedReader 接口](../diagnostics/isymunmanagedreader-interface.md)) 创建符号读取器。|  
  
## <a name="remarks"></a>注解  

 此接口以逻辑方式扩展了 "ICorDebugModule" 和 "ICorDebugModule2" 接口。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 4.5、4、3.5 SP1
  
## <a name="see-also"></a>另请参阅

- [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)
- [ICorDebug 接口](icordebug-interface.md)

- [调试接口](debugging-interfaces.md)
