---
title: ICorDebugFunction::GetCurrentVersionNumber 方法
ms.date: 03/30/2017
api_name:
- ICorDebugFunction.GetCurrentVersionNumber
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugFunction::GetCurrentVersionNumber
helpviewer_keywords:
- GetCurrentVersionNumber method [.NET Framework debugging]
- ICorDebugFunction::GetCurrentVersionNumber method [.NET Framework debugging]
ms.assetid: c3af1575-cbe6-457a-bc08-c53460edcbc8
topic_type:
- apiref
ms.openlocfilehash: 14579d4c84be9bb225e618715b3a7d45ccaac0a9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728142"
---
# <a name="icordebugfunctiongetcurrentversionnumber-method"></a>ICorDebugFunction::GetCurrentVersionNumber 方法

获取对此 ICorDebugFunction 对象所表示的函数进行的最新编辑的版本号。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCurrentVersionNumber (  
    [out] ULONG32 *pnCurrentVersion  
);  
```  
  
## <a name="parameters"></a>参数  

 `pnCurrentVersion`  
 弄指向整数值的指针，该整数值是对此函数进行的最新编辑的版本号。  
  
## <a name="remarks"></a>注解  

 对此函数进行的最新编辑的版本号可能大于函数本身的版本号。 使用 [ICorDebugFunction2：： GetVersionNumber](icordebugfunction2-getversionnumber-method.md) 方法或 [ICorDebugCode：： GetVersionNumber](icordebugcode-getversionnumber-method.md) 方法检索函数的版本号。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
