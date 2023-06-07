---
title: ICorDebugType 接口
ms.date: 03/30/2017
api_name:
- ICorDebugType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType
helpviewer_keywords:
- ICorDebugType interface [.NET Framework debugging]
ms.assetid: 94e02e31-67ea-4b00-8148-a46740a4571d
topic_type:
- apiref
ms.openlocfilehash: 9407dda7aab337f667cd5043b562d0eac94f0f04
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711918"
---
# <a name="icordebugtype-interface"></a>ICorDebugType 接口

表示基本或复杂 (的类型，即用户定义的) 。 如果该类型是泛型类型，则 `ICorDebugType` 表示未实例化的泛型类型。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnumerateTypeParameters 方法](icordebugtype-enumeratetypeparameters-method.md)|获取一个接口指针，该指针指向引用 <xref:System.Type> 此所引用类的泛型参数的 ICorDebugTypeEnum `ICorDebugType` 。|  
|[GetBase 方法](icordebugtype-getbase-method.md)|获取一个接口指针，该指针指向 `ICorDebugType` 引用此引用的类 `ICorDebugType` （如果存在此类）的基类。|  
|[GetClass 方法](icordebugtype-getclass-method.md)|获取一个接口指针，该指针指向引用此的类型化构造函数的 ICorDebugClass `ICorDebugType` 。|  
|[GetFirstTypeParameter 方法](icordebugtype-getfirsttypeparameter-method.md)|获取一个接口指针 `ICorDebugType` ，该指针指向引用此引用的类的构造函数的第一个泛型 <xref:System.Type> 参数 `ICorDebugType` 。|  
|[GetRank 方法](icordebugtype-getrank-method.md)|获取数组类型中的维度数。|  
|[GetStaticFieldValue 方法](icordebugtype-getstaticfieldvalue-method.md)|获取一个指向 ICorDebugValue 的接口指针，该指针包含指定的堆栈帧中指定字段标记所引用的静态字段的值。|  
|[GetType 方法](icordebugtype-gettype-method.md)|获取一个 CorElementType 值，该值描述此所引用的公共语言运行时的本机类型 <xref:System.Type> `ICorDebugType` 。|  
  
## <a name="remarks"></a>注解  

 如果类型为泛型，则 `ICorDebugClass` 表示未实例化的类型。 `ICorDebugType`接口表示一个实例化的泛型类型。 例如，Hashtable 由 \<K, V> 表示 `ICorDebugClass` ，而哈希表将由 \<Int32, String> 表示 `ICorDebugType` 。  
  
 非泛型类型由 `ICorDebugClass` 和表示 `ICorDebugType` 。 后一种接口在 .NET Framework 版本2.0 中引入，用于处理类型实例化。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
