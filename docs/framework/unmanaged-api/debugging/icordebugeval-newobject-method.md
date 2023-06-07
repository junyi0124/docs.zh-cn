---
title: ICorDebugEval::NewObject 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval.NewObject
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::NewObject
helpviewer_keywords:
- NewObject method [.NET Framework debugging]
- ICorDebugEval::NewObject method [.NET Framework debugging]
ms.assetid: ce3025e8-defa-4c5e-8298-f49d71fa5736
topic_type:
- apiref
ms.openlocfilehash: a4d6dd0df9f38561ab5014d7ab65fde6793c9846
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729741"
---
# <a name="icordebugevalnewobject-method"></a>ICorDebugEval::NewObject 方法

分配一个新的对象实例，并调用指定的构造函数方法。  
  
 此方法在 .NET Framework 版本2.0 中已过时。 改 [为使用 ICorDebugEval2：： NewParameterizedObject](icordebugeval2-newparameterizedobject-method.md) 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT NewObject (  
    [in] ICorDebugFunction  *pConstructor,  
    [in] ULONG32            nArgs,  
    [in, size_is(nArgs)] ICorDebugValue *ppArgs[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `pConstructor`  
 中要调用的构造函数。  
  
 `nArgs`  
 [in] `ppArgs` 数组的大小。  
  
 `ppArgs`  
 中ICorDebugValue 对象的数组，其中每个对象都表示要传递给构造函数的参数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：** 1.1、1。0  
  
## <a name="see-also"></a>另请参阅

- [NewParameterizedObject 方法](icordebugeval2-newparameterizedobject-method.md)
