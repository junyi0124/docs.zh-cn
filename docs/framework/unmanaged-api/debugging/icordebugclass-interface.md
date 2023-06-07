---
title: ICorDebugClass 接口
ms.date: 03/30/2017
api_name:
- ICorDebugClass
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass
helpviewer_keywords:
- ICorDebugClass interface [.NET Framework debugging]
ms.assetid: 03a6facb-f12f-49be-9839-e73b9c791cd5
topic_type:
- apiref
ms.openlocfilehash: 4f488741f4233f06c128e0a262ce798ef27af3ff
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699620"
---
# <a name="icordebugclass-interface"></a>ICorDebugClass 接口

表示基类型或复杂类型（即用户定义的类型）。 如果该类型为泛型类型，则 `ICorDebugClass` 表示实例化的泛型类型。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetModule 方法](icordebugclass-getmodule-method.md)|获取定义此类的模块。|  
|[GetStaticFieldValue 方法](icordebugclass-getstaticfieldvalue-method.md)|获取指定的静态字段的值。|  
|[GetToken 方法](icordebugclass-gettoken-method.md)|获取 `TypeDef` 此类的元数据标记。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugClass`接口表示未实例化的泛型类型。 ICorDebugType 接口表示一个实例化的泛型类型。 例如，将由 `Hashtable<K, V>` 表示 `ICorDebugClass` ，而将由 `Hashtable<Int32, String>` 表示 `ICorDebugType` 。  
  
 非泛型类型由 `ICorDebugClass` 和表示 `ICorDebugType` 。 后一种接口在 .NET Framework 版本2.0 中引入，用于处理类型实例化。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
