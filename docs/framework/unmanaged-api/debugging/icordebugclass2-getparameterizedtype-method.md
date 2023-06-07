---
title: ICorDebugClass2::GetParameterizedType 方法
ms.date: 03/30/2017
api_name:
- ICorDebugClass2.GetParameterizedType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass2::GetParameterizedType
helpviewer_keywords:
- GetParameterizedType method [.NET Framework debugging]
- ICorDebugClass2::GetParameterizedType method [.NET Framework debugging]
ms.assetid: 94b591c4-9302-4af2-a510-089496afb036
topic_type:
- apiref
ms.openlocfilehash: 139181975d16c2cdacec10ed646cfc2b8fb31a20
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717989"
---
# <a name="icordebugclass2getparameterizedtype-method"></a>ICorDebugClass2::GetParameterizedType 方法

获取此类的类型声明。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetParameterizedType (  
    [in] CorElementType                      elementType,  
    [in] ULONG32                             nTypeArgs,  
    [in, size_is(nTypeArgs)] ICorDebugType  *ppTypeArgs[],  
    [out] ICorDebugType                    **ppType  
);  
```  
  
## <a name="parameters"></a>参数  

 `elementType`  
 中为此类指定元素类型的 CorElementType 枚举的值：如果此 ICorDebugClass2 表示值类型，请将此值设置为 ELEMENT_TYPE_VALUETYPE。 如果此表示复杂类型，则将此值设置为 ELEMENT_TYPE_CLASS `ICorDebugClass2` 。  
  
 `nTypeArgs`  
 中如果类型为泛型，则为类型参数的数目。 如果任何) 必须与类所需的数目相匹配，则 (类型参数的数目。  
  
 `ppTypeArgs`  
 中指针的数组，其中每个都指向表示类型参数的 ICorDebugType 对象。 如果类为非泛型类，则此值为 null。  
  
 `ppType`  
 弄指向表示类型声明的对象的地址的指针 `ICorDebugType` 。 此对象等效于 <xref:System.Type> 托管代码中的对象。  
  
## <a name="remarks"></a>注解  

 如果类是非泛型类（即，如果它没有类型参数）， `GetParameterizedType` 只需获取与类相对应的运行时类型对象。 `elementType`如果类是值类型，则应将参数设置为类的正确元素类型： ELEMENT_TYPE_VALUETYPE; 否则 ELEMENT_TYPE_CLASS。  
  
 如果类接受类型参数 (例如 `ArrayList<T>`) ，则可以使用 `GetParameterizedType` 为实例化的类型（如）构造类型对象 `ArrayList<int>` 。  
  
## <a name="background-information"></a>背景信息  

 在 .NET Framework 版本1.0 和1.1 中，元数据中的每个类型都可以直接映射到正在运行的进程中的类型。 因此，元数据类型和运行时类型在正在运行的进程中具有单个表示形式。 但是，元数据中的一种泛型类型可以映射到正在运行的进程中类型的许多不同的实例化。 例如，元数据类型 `SortedList<K,V>` 可以映射到、、等 `SortedList<String, EmployeeRecord>` `SortedList<Int32, String>` `SortedList<String,Array<Int32>>` 。 因此，您需要一种方法来处理类型实例化。  
  
 .NET Framework 版本2.0 引入了 `ICorDebugType` 接口。 对于泛型类型， `ICorDebugClass` 或 `ICorDebugClass2` 对象表示未实例化的类型 (`SortedList<K,V>`) ，而 `ICorDebugType` 对象表示各种实例化的类型。 给定 `ICorDebugClass` 或 `ICorDebugClass2` 对象，可以 `ICorDebugType` 通过调用方法为任何实例化创建对象 `ICorDebugClass2::GetParameterizedType` 。 还可以为 `ICorDebugType` 简单类型（如 Int32）或非泛型类型创建对象。  
  
 引入 `ICorDebugType` 对象来表示类型的运行时概念会在整个 API 中产生波纹效果。 以前用过 `ICorDebugClass` 或 `ICorDebugClass2` 对象甚至值的函数 `CorElementType` 被通用化为采用 `ICorDebugType` 对象。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
