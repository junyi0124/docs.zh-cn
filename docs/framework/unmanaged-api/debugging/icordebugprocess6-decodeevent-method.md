---
title: ICorDebugProcess6::DecodeEvent 方法
ms.date: 03/30/2017
ms.assetid: 1453bc0c-6e0d-4d5a-b176-22607f8a3e6c
ms.openlocfilehash: ed75b3c5657fed805f187285a576b81598be331c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95690273"
---
# <a name="icordebugprocess6decodeevent-method"></a>ICorDebugProcess6::DecodeEvent 方法

对封装于特殊构造的本机异常调试事件有效载荷中的托管调试事件进行解码。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DecodeEvent(  
        [in, length_is(countBytes), size_is(countBytes)]  const BYTE pRecord[],  
        [in] DWORD countBytes,  
        [in] CorDebugRecordFormat format,  
        [in] DWORD dwFlags,
        [in] DWORD dwThreadId,
        [out] ICorDebugDebugEvent **ppEvent  
);  
```  
  
## <a name="parameters"></a>参数  

 `pRecord`  
 [输入] 包含托管调试事件相关信息的本机异常调试事件中的字节数组指针。  
  
 `countBytes`  
 [输入] `pRecord` 字节数组中的元素数量。  
  
 `format`  
 中 [CorDebugRecordFormat](cordebugrecordformat-enumeration.md) 枚举成员，用于指定非托管调试事件的格式。  
  
 `dwFlags`  
 [输入] 位域依赖于目标体系结构并指定调试事件相关的其他信息。 对于 Windows 系统，它可以是 [CorDebugDecodeEventFlagsWindows](cordebugdecodeeventflagswindows-enumeration.md) 枚举的成员。  
  
 `dwThreadId`  
 [输入] 引发异常的线程的操作系统识别符。  
  
 `ppEvent`  
 弄指向表示已解码的托管调试事件的 [ICorDebugDebugEvent](icordebugdebugevent-interface.md) 对象地址的指针。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试进程6”接口](icordebugprocess6-interface.md)
- [调试接口](debugging-interfaces.md)
