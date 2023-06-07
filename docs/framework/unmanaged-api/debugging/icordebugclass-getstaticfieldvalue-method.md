---
title: ICorDebugClass::GetStaticFieldValue 方法
ms.date: 03/30/2017
api_name:
- ICorDebugClass.GetStaticFieldValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass::GetStaticFieldValue
helpviewer_keywords:
- GetStaticFieldValue method, ICorDebugClass interface [.NET Framework debugging]
- ICorDebugClass::GetStaticFieldValue method [.NET Framework debugging]
ms.assetid: 56e718b4-fabd-418b-a5b3-3cc33c745683
topic_type:
- apiref
ms.openlocfilehash: dd1608badf553650b05b7de98d9bbcd76b2f3edf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728428"
---
# <a name="icordebugclassgetstaticfieldvalue-method"></a>ICorDebugClass::GetStaticFieldValue 方法

获取指定的静态字段的值。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetStaticFieldValue (  
    [in]  mdFieldDef         fieldDef,  
    [in]  ICorDebugFrame     *pFrame,  
    [out] ICorDebugValue     **ppValue  
);  
```  
  
## <a name="parameters"></a>参数  

 `fieldDef`  
 中 `Def` 引用要检索的字段的字段标记。  
  
 `pFrame`  
 中指向 ICorDebugFrame 对象的指针，该对象表示要用于区分线程、上下文或应用程序域静态对象的帧。  
  
 如果静态字段相对于线程、上下文或应用程序域，则该框架将确定正确的值。  
  
 `ppValue`  
 弄指向 ICorDebugValue 对象的地址的指针，该对象表示静态字段的值。  
  
## <a name="remarks"></a>注解  

 对于参数化类型，静态字段的值是相对于特定实例化的。 因此，如果类构造函数采用类型的参数 <xref:System.Type> ，请调用 [ICorDebugType：： GetStaticFieldValue](icordebugtype-getstaticfieldvalue-method.md) 而不是 `ICorDebugClass::GetStaticFieldValue` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
