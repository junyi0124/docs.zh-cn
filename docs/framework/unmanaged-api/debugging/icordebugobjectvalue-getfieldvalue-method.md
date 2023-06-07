---
title: ICorDebugObjectValue::GetFieldValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugObjectValue.GetFieldValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugObjectValue::GetFieldValue
helpviewer_keywords:
- ICorDebugObjectValue::GetFieldValue method [.NET Framework debugging]
- GetFieldValue method [.NET Framework debugging]
ms.assetid: c96770b0-3e09-47bb-bd29-20353b043459
topic_type:
- apiref
ms.openlocfilehash: 745be25183f6b94e7a807c4230961d72e2836fe5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695330"
---
# <a name="icordebugobjectvaluegetfieldvalue-method"></a>ICorDebugObjectValue::GetFieldValue 方法

获取此对象值的指定类的指定字段的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetFieldValue (  
    [in]  ICorDebugClass     *pClass,  
    [in]  mdFieldDef         fieldDef,  
    [out] ICorDebugValue     **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `pClass`  
 中一个指向 "ICorDebugClass" 对象的指针，该对象表示要获取其字段值的类。  
  
 `fieldDef`  
 中 `mdFieldDef` 引用描述字段的元数据的令牌。  
  
 `ppValue`  
 弄一个指向 "ICorDebugValue" 对象的指针，该对象表示指定字段的值。  
  
## <a name="remarks"></a>注解  

 参数中指定的类 `pClass` 必须位于对象值的类的层次结构中，并且字段必须是该类的字段。  
  
 `GetFieldValue`对于泛型对象和泛型类，该方法仍将成功。 例如，如果 MyDictionary \<V> 从字典继承 \<string,V> ，并且对象值的类型为 MyDictionary，则 \<int32> 传递字典的 `ICorDebugClass` 对象 \<K,V> 将成功获取字典的字段 \<string,int32> 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
