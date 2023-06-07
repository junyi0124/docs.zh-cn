---
title: ICorDebugMemoryBuffer::GetStartAddress 方法
ms.date: 03/30/2017
ms.assetid: f804d9ab-8c88-44f0-b278-5fcca7f87726
ms.openlocfilehash: f76bf1479db987e4956d8b876f67d629d927f956
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710746"
---
# <a name="icordebugmemorybuffergetstartaddress-method"></a>ICorDebugMemoryBuffer::GetStartAddress 方法

获取内存缓冲区的起始地址。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetStartAddress(  
   [out] LPCVOID *address  
);  
```  
  
## <a name="parameters"></a>参数  

 `address`  
 [out] 指向内存缓冲区起始地址的指针。  
  
## <a name="remarks"></a>注解  
  
> [!WARNING]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugMemoryBuffer 接口](icordebugmemorybuffer-interface.md)
- [调试接口](debugging-interfaces.md)
