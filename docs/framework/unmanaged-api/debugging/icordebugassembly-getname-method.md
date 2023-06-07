---
title: ICorDebugAssembly::GetName 方法
ms.date: 03/30/2017
api_name:
- ICorDebugAssembly.GetName
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAssembly::GetName
helpviewer_keywords:
- ICorDebugAssembly::GetName method [.NET Framework debugging]
- GetName method, ICorDebugAssembly interface [.NET Framework debugging]
ms.assetid: cdeda721-b214-4503-a291-c70b68b5f36b
topic_type:
- apiref
ms.openlocfilehash: 3794a3b308bd5c96a38337d8b81e61167e4dc988
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734044"
---
# <a name="icordebugassemblygetname-method"></a>ICorDebugAssembly::GetName 方法

获取此实例表示的程序集的名称 `ICorDebugAssembly` 。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetName (  
    [in] ULONG32  cchName,  
    [out] ULONG32 *pcchName,  
    [out, size_is(cchName), length_is(*pcchName)] WCHAR szName[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cchName`  
 [in] `szName` 数组的大小。  
  
 `pcchName`  
 弄指向整数的指针，该整数指定名称的实际长度。  
  
 `szName`  
 弄存储名称的数组。  
  
## <a name="remarks"></a>注解  

 `GetName`方法返回程序集的完整路径和文件名。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
