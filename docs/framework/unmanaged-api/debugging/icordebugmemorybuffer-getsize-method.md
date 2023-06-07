---
title: ICorDebugMemoryBuffer::GetSize 方法
ms.date: 03/30/2017
ms.assetid: 9ffd5482-268e-4680-9fd1-bfb0b7d66450
ms.openlocfilehash: 7f5458dd12ca83c1a5190bbf7fab0f8e5d06a0e1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710745"
---
# <a name="icordebugmemorybuffergetsize-method"></a>ICorDebugMemoryBuffer::GetSize 方法

获取内存缓冲区的大小（以字节为单位）。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetSize(  
   [out] ULONG32 *pcbBufferLength  
);  
```  
  
## <a name="parameters"></a>参数  

 `pcbBufferLength`  
 [out] 指向内存缓冲区大小的指针。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMemoryBuffer 接口](icordebugmemorybuffer-interface.md)
- [调试接口](debugging-interfaces.md)
