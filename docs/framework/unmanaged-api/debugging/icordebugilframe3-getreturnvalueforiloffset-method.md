---
title: ICorDebugILFrame3::GetReturnValueForILOffset 方法
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
api_name:
- ICorDebugILFrame3.GetReturnValueForILOffset
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: 06522727-5f64-4391-9331-11386883c352
topic_type:
- apiref
ms.openlocfilehash: 11207298b071527151535144330790df767c2101
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724996"
---
# <a name="icordebugilframe3getreturnvalueforiloffset-method"></a>ICorDebugILFrame3::GetReturnValueForILOffset 方法

获取一个 "ICorDebugValue" 对象，该对象封装函数的返回值。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT GetReturnValueForILOffset(  
    ULONG32 ILoffset,
    [out] ICorDebugValue **ppReturnValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `ILOffset`  
 IL 偏移量。 请参阅“备注”部分。  
  
 `ppReturnValue`  
 一个指向 "ICorDebugValue" 接口对象地址的指针，该对象提供有关函数调用的返回值的信息。  
  
## <a name="remarks"></a>注解  

 此方法与 [ICorDebugCode3：： GetReturnValueLiveOffset](icordebugcode3-getreturnvalueliveoffset-method.md) 方法一起使用，以获取方法的返回值。 这对于返回值被忽略的方法特别有用，如以下两个代码示例所示。 第一个示例调用 <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> 方法，但忽略方法的返回值。  
  
 [!code-csharp[Unmanaged.Debugging.MRV#1](../../../../samples/snippets/csharp/VS_Snippets_CLR/unmanaged.debugging.mrv/cs/mrv1.cs#1)]
 [!code-vb[Unmanaged.Debugging.MRV#1](../../../../samples/snippets/visualbasic/VS_Snippets_CLR/unmanaged.debugging.mrv/vb/mrv1.vb#1)]  
  
 第二个示例演示了调试中更常见的问题。 因为方法在方法调用中用作参数，所以仅当调试器逐步执行调用的方法时，才能访问其返回值。 在许多情况下，尤其是在外部库中定义了所调用的方法时，这是不可能的。  
  
 [!code-csharp[Unmanaged.Debugging.MRV#2](../../../../samples/snippets/csharp/VS_Snippets_CLR/unmanaged.debugging.mrv/cs/mrv2.cs#2)]
 [!code-vb[Unmanaged.Debugging.MRV#2](../../../../samples/snippets/visualbasic/VS_Snippets_CLR/unmanaged.debugging.mrv/vb/mrv2.vb#2)]  
  
 如果将 [ICorDebugCode3：： GetReturnValueLiveOffset](icordebugcode3-getreturnvalueliveoffset-method.md) 方法传递给函数调用站点的 IL 偏移量，它将返回一个或多个本机偏移量。 然后，调试器可以在函数中的这些本机偏移量上设置断点。 当调试器遇到其中一个断点时，您可以传递您传递给此方法的同一 IL 偏移量来获取返回值。 调试器随后应清除它所设置的所有断点。  
  
> [!WARNING]
> [ICorDebugCode3：： GetReturnValueLiveOffset 方法](icordebugcode3-getreturnvalueliveoffset-method.md)和 `ICorDebugILFrame3::GetReturnValueForILOffset` 方法仅允许获取引用类型的返回值信息。 从值类型检索返回值信息 (也就是说，不支持从) 派生的所有类型 <xref:System.ValueType> 。  
  
 参数指定的 IL 偏移量 `ILOffset` 应位于函数调用站点上，并且调试对象应在 [ICorDebugCode3：： GetReturnValueLiveOffset](icordebugcode3-getreturnvalueliveoffset-method.md) 方法为同一 IL 偏移量返回的本机偏移量处停止。 如果在指定的 IL 偏移量的正确位置没有停止调试对象，则 API 将失败。  
  
 如果函数调用不返回值，则 API 将失败。  
  
 `ICorDebugILFrame3::GetReturnValueForILOffset`方法仅适用于基于 x86 的和 AMD64 系统。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v451plus](../../../../includes/net-current-v451plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetReturnValueLiveOffset 方法](icordebugcode3-getreturnvalueliveoffset-method.md)
- [ICorDebugILFrame3 接口](icordebugilframe3-interface.md)
