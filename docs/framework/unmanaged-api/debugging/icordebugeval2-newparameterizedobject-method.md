---
title: ICorDebugEval2::NewParameterizedObject 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval2.NewParameterizedObject
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval2::NewParameterizedObject
helpviewer_keywords:
- NewParameterizedObject method [.NET Framework debugging]
- ICorDebugEval2::NewParameterizedObject method [.NET Framework debugging]
ms.assetid: 3d705463-e640-4249-8036-4e8206d03cfe
topic_type:
- apiref
ms.openlocfilehash: 8c91296bd4185fd98962d49f611a3cdcb5f0ad28
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729650"
---
# <a name="icordebugeval2newparameterizedobject-method"></a>ICorDebugEval2::NewParameterizedObject 方法

实例化一个新的参数化类型对象，并调用该对象的构造函数方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NewParameterizedObject (  
    [in] ICorDebugFunction     *pConstructor,  
    [in] ULONG32               nTypeArgs,  
    [in, size_is(nTypeArgs)] ICorDebugType *ppTypeArgs[],  
    [in] ULONG32               nArgs,  
    [in, size_is(nArgs)] ICorDebugValue *ppArgs[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `pConstructor`  
 中指向 ICorDebugFunction 对象的指针，该对象表示要实例化的对象的构造函数。  
  
 `nTypeArgs`  
 中传递的类型参数的数目。  
  
 `ppTypeArgs`  
 中指针的数组，其中每个都指向一个 ICorDebugType 对象，该对象表示要实例化的对象的类型参数。  
  
 `nArgs`  
 中传递给构造函数的参数的数目。  
  
 `ppArgs`  
 中指针的数组，其中每个都指向表示传递给构造函数的参数值的 ICorDebugValue 对象。  
  
## <a name="remarks"></a>注解  

 对象的构造函数可以接受 <xref:System.Type> 参数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
