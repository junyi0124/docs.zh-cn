---
title: ICorDebugCode 接口
ms.date: 03/30/2017
api_name:
- ICorDebugCode
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode
helpviewer_keywords:
- ICorDebugCode interface [.NET Framework debugging]
ms.assetid: 7bd14fb6-8b54-4484-a891-e3c21859c019
topic_type:
- apiref
ms.openlocfilehash: 03cbc1a598ba6c0166f72ff404c239763956c996
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687601"
---
# <a name="icordebugcode-interface"></a>ICorDebugCode 接口

表示 Microsoft 中间语言 (MSIL) 代码段或本机代码段。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[CreateBreakpoint 方法](icordebugcode-createbreakpoint-method.md)|按指定的偏移量创建断点。|  
|[GetAddress 方法](icordebugcode-getaddress-method.md)|获取此表示的代码段 (RVA) 的相对虚拟地址 `ICorDebugCode` 。|  
|[GetCode 方法](icordebugcode-getcode-method.md)|获取用于反汇编的指定函数的所有代码。 此方法已弃用;改 [为使用 ICorDebugCode2：： GetCodeChunks](icordebugcode2-getcodechunks-method.md) 。|  
|[GetEnCRemapSequencePoints 方法](icordebugcode-getencremapsequencepoints-method.md)|未实现。|  
|[GetFunction 方法](icordebugcode-getfunction-method.md)|获取与此关联的 "ICorDebugFunction" `ICorDebugCode` 。|  
|[GetILToNativeMapping 方法](icordebugcode-getiltonativemapping-method.md)|获取 "COR_DEBUG_IL_TO_NATIVE_MAP" 实例的数组，这些实例表示从 MSIL 偏移量到本机偏移量的映射。|  
|[GetSize 方法](icordebugcode-getsize-method.md)|获取此表示的二进制代码的大小（以字节为单位） `ICorDebugCode` 。|  
|[GetVersionNumber 方法](icordebugcode-getversionnumber-method.md)|获取一个从1开始的数字，该数字标识此表示的代码的版本 `ICorDebugCode` 。|  
|[IsIL 方法](icordebugcode-isil-method.md)|获取一个值，该值指示是否 `ICorDebugCode` 在 MSIL 中编译此。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugCode` 可以表示 MSIL 或本机代码。 表示 MSIL 代码的 "ICorDebugFunction" 对象可以有零个或一个 `ICorDebugCode` 关联的对象。 表示本机代码的 "ICorDebugFunction" 对象可以有任意数量的 `ICorDebugCode` 关联对象。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugCode3 接口](icordebugcode3-interface.md)
- [调试接口](debugging-interfaces.md)
