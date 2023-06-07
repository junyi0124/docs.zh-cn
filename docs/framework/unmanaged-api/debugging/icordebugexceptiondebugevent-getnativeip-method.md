---
title: ICorDebugExceptionDebugEvent::GetNativeIP 方法
ms.date: 03/30/2017
ms.assetid: 12e6a262-d9ac-49b8-9b80-1e653a2a3819
ms.openlocfilehash: f3b29b3ceda521afe9543588af332531aa03e84e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697410"
---
# <a name="icordebugexceptiondebugeventgetnativeip-method"></a>ICorDebugExceptionDebugEvent::GetNativeIP 方法

获取此异常调试事件的本机指令指针。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetNativeIP(  
   [out]CORDB_ADDRESS *pIP  
);  
```  
  
## <a name="parameters"></a>参数  

 `pIP`  
 [out] 指向此异常调试事件的指令指针的指针。 有关详细信息，请参阅备注部分。  
  
## <a name="remarks"></a>注解  

 此指令指针的含义取决于事件类型，如下表所示。  
  
|事件类型|`pStackPointer` 值的含义|  
|----------------|--------------------------------------|  
|[MANAGED_EXCEPTION_FIRST_CHANCE](cordebugrecordformat-enumeration.md)|出错指令的地址。|  
|[MANAGED_EXCEPTION_USER_FIRST_CHANCE](cordebugrecordformat-enumeration.md)|[GetStackPointer](icordebugexceptiondebugevent-getstackpointer-method.md)方法指示的帧中的代码地址，如果未引发异常，则执行将继续执行。 此异常可能会，也可能不会导致在此帧中执行的不同代码（例如，`try/catch/finally` 子句的 catch 块）。|  
|[MANAGED_EXCEPTION_CATCH_HANDLER_FOUND](cordebugrecordformat-enumeration.md)|`catch`处理程序执行将在[GetStackPointer](icordebugexceptiondebugevent-getstackpointer-method.md)方法指示的帧中开始的代码地址。|  
|[MANAGED_EXCEPTION_UNHANDLED](cordebugrecordformat-enumeration.md)|`pIP` 为 0。|  
  
 可以从 [ICorDebugDebugEvent：： GetEventKind](icordebugdebugevent-geteventkind-method.md) 方法获取事件类型。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试异常调试事件”接口](icordebugexceptiondebugevent-interface.md)
- [调试接口](debugging-interfaces.md)
