---
title: ICorDebugAppDomain2::GetArrayOrPointerType 方法
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain2.GetArrayOrPointerType
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain2::GetArrayOrPointerType
helpviewer_keywords:
- GetArrayOrPointerType method [.NET Framework debugging]
- ICorDebugAppDomain2::GetArrayOrPointerType method [.NET Framework debugging]
ms.assetid: 97e493f5-3a62-4ec7-b42f-4af57bf71f57
topic_type:
- apiref
ms.openlocfilehash: 8b3f6ae92e39f5385bf29f8b29abbb1726136088
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724762"
---
# <a name="icordebugappdomain2getarrayorpointertype-method"></a>ICorDebugAppDomain2::GetArrayOrPointerType 方法

获取指定类型的数组，或指定类型的指针或引用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetArrayOrPointerType (  
    [in]  CorElementType    elementType,  
    [in]  ULONG32           nRank,  
    [in]  ICorDebugType     *pTypeArg,  
    [out] ICorDebugType     **ppType  
);  
```  
  
## <a name="parameters"></a>参数  

 `elementType`  
 中CorElementType 枚举的一个值，指定要创建的数组、指针或引用)  (基础本机类型。  
  
 `nRank`  
 中秩 (为数组的维数) 。 如果 `elementType` 指定指针或引用类型，则此值必须为0。  
  
 `pTypeArg`  
 中指向 ICorDebugType 对象的指针，该对象表示要创建的数组、指针或引用的类型。  
  
 `ppType`  
 弄指向对象地址的指针 `ICorDebugType` ，该对象表示构造的数组、指针类型或引用类型。  
  
## <a name="remarks"></a>注解  

 *ElementType* 的值必须是下列值之一：  
  
- ELEMENT_TYPE_PTR  
  
- ELEMENT_TYPE_BYREF  
  
- ELEMENT_TYPE_ARRAY 或 ELEMENT_TYPE_SZARRAY  
  
 如果 *elementType* 的值是 ELEMENT_TYPE_PTR 或 ELEMENT_TYPE_BYREF，则 *nRank* 必须为零。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
