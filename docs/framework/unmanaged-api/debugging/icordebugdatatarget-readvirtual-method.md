---
title: ICorDebugDataTarget::ReadVirtual 方法
ms.date: 03/30/2017
api_name:
- ICorDebugDataTarget.ReadVirtual Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugDataTarget::ReadVirtual
helpviewer_keywords:
- ICorDebugDataTarget::ReadVirtual method [.NET Framework debugging]
- ReadVirtual method, ICorDebugDataTarget interface [.NET Framework debugging]
ms.assetid: 55e57640-b3d2-413d-b4f4-fbc27fb8e37c
topic_type:
- apiref
ms.openlocfilehash: 8fb0cfc72867653eaff65f3183dacf9191604290
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679723"
---
# <a name="icordebugdatatargetreadvirtual-method"></a>ICorDebugDataTarget::ReadVirtual 方法

获取从指定地址开始的连续内存块，并将其返回到提供的缓冲区中。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ReadVirtual(  
    [in] CORDB_ADDRESS   address,  
    [out, size_is(bytesRequested), length_is(*pBytesRead)]  
          BYTE *     pBuffer,  
    [in]  ULONG32    bytesRequested,  
    [out] ULONG32 *  pBytesRead);  
```  
  
## <a name="parameters"></a>参数  

 `address`  
 中请求的内存的开始地址。  
  
 `pbuffer`  
 弄内存将存储到的缓冲区。  
  
 `bytesRequested`  
 中要从目标地址获取的字节数。  
  
 `pBytesRead`  
 弄实际从目标地址中读取的字节数。 该值可以少于 `bytesRequested` 。  
  
## <a name="remarks"></a>注解  

 如果可以读取指定开始地址处的第一个字节 () ，则调用应返回成功 (，以支持通过自描述长度有效读取数据结构，如) 以 null 结尾的字符串。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugDataTarget 接口](icordebugdatatarget-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
