---
title: ICorDebugReferenceValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugReferenceValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugReferenceValue
helpviewer_keywords:
- ICorDebugReferenceValue interface [.NET Framework debugging]
ms.assetid: 2040e2be-119a-4cfb-ae52-b0b6f052665c
topic_type:
- apiref
ms.openlocfilehash: 343e504e086e740236d7b5977452cc0d789883fc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728402"
---
# <a name="icordebugreferencevalue-interface"></a>ICorDebugReferenceValue 接口

提供管理作为对对象的引用的值的方法。  (即，此接口提供管理指针的方法。 ) 此接口实现 "ICorDebugValue"。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Dereference 方法](icordebugreferencevalue-dereference-method.md)|获取所引用的对象。|  
|[DereferenceStrong 方法](icordebugreferencevalue-dereferencestrong-method.md)|未实现。 请勿调用此方法。|  
|[GetValue 方法](icordebugreferencevalue-getvalue-method.md)|获取所引用对象的当前内存地址。|  
|[IsNull 方法](icordebugreferencevalue-isnull-method.md)|获取一个值，该值指示此 `ICorDebugReferenceValue` 值是否为 null 值，在这种情况下，不 `ICorDebugReferenceValue` 指向对象。|  
|[SetValue 方法](icordebugreferencevalue-setvalue-method.md)|设置当前内存地址。 也就是说，此方法会将此设置 `ICorDebugReferenceValue` 为指向对象。|  
  
## <a name="remarks"></a>注解  

 在继续调试进程时，公共语言运行时 (CLR) 可以对对象进行垃圾回收。 垃圾回收可能会在内存中移动对象。 `ICorDebugReferenceValue`将与垃圾回收一起合作，以便在垃圾回收后更新其信息，或者在垃圾回收之前隐式地将其无效。  
  
 `ICorDebugReferenceValue`继续调试的进程后，对象可能会隐式失效。 派生的 "ICorDebugHandleValue" 在显式发布或公开之前不会失效。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
