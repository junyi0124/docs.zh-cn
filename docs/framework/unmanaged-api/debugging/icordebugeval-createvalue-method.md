---
title: ICorDebugEval::CreateValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval.CreateValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::CreateValue
helpviewer_keywords:
- ICorDebugEval::CreateValue method [.NET Framework debugging]
- CreateValue method [.NET Framework debugging]
ms.assetid: 9a1c0b47-6f10-4fcb-844a-4ab2d7990140
topic_type:
- apiref
ms.openlocfilehash: 41db6ac00d8646651d0e8433d076c37af6020071
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95696149"
---
# <a name="icordebugevalcreatevalue-method"></a>ICorDebugEval::CreateValue 方法

创建指定类型的值，其初始值为零或 null。  
  
 此方法在 .NET Framework 版本2.0 中已过时。 改 [为使用 ICorDebugEval2：： CreateValueForType](icordebugeval2-createvaluefortype-method.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateValue (  
    [in] CorElementType     elementType,  
    [in] ICorDebugClass     *pElementClass,  
    [out] ICorDebugValue    **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `elementType`  
 中 [CorElementType](../metadata/corelementtype-enumeration.md) 枚举的一个值，该值指定值的类型。  
  
 `pElementClass`  
 中指向 [ICorDebugClass](icordebugclass-interface.md) 对象的指针，该对象指定值的类（如果该类型不是基元类型）。  
  
 `ppValue`  
 弄指向表示值的 "ICorDebugValue" 对象地址的指针。  
  
## <a name="remarks"></a>注解  

 `CreateValue` 创建 `ICorDebugValue` 给定类型的对象，其唯一目的是在函数计算中使用它。 此值对象可用于将用户常数作为参数传递。  
  
 如果值的类型为基元类型，则其初始值为零或 null。 使用 [ICorDebugGenericValue：： SetValue](icordebuggenericvalue-setvalue-method.md) 设置基元类型的值。  
  
 如果 ELEMENT_TYPE_CLASS 的值 `elementType` ，则将获取 `ppValue`) 表示 null 对象引用的 "ICorDebugReferenceValue" (。 您可以使用此对象将 null 传递给具有对象引用参数的函数求值。 不能将设置 `ICorDebugValue` 为任何内容; 它始终保留为 null。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1.1、1。0  
  
## <a name="see-also"></a>另请参阅

- [CreateValueForType 方法](icordebugeval2-createvaluefortype-method.md)
- [ICorDebugEval 接口](icordebugeval-interface.md)
