---
title: ICorDebugEval::Abort 方法
ms.date: 03/30/2017
api_name:
- ICorDebugEval.Abort
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugEval::Abort
helpviewer_keywords:
- Abort method, ICorDebugEval interface [.NET Framework debugging]
- ICorDebugEval::Abort method [.NET Framework debugging]
ms.assetid: 7070b6d0-f2e0-44ff-b124-0944cd807e69
topic_type:
- apiref
ms.openlocfilehash: 6e6ea5e42c5e1b1943a080ae02e1dbf6d702bebc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705847"
---
# <a name="icordebugevalabort-method"></a>ICorDebugEval::Abort 方法

中止此 ICorDebugEval 对象当前正在执行的计算。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Abort ();  
```  
  
## <a name="remarks"></a>备注  

 如果计算是嵌套的，并且不是最新的，则该 `Abort` 方法可能会失败。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
