---
title: ICorDebugProcessEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugProcessEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcessEnum::Next
helpviewer_keywords:
- Next method, ICorDebugProcessEnum interface [.NET Framework debugging]
- ICorDebugProcessEnum::Next method [.NET Framework debugging]
ms.assetid: 4ac7077c-8d88-49c4-b360-b3af0c541c63
topic_type:
- apiref
ms.openlocfilehash: 6aee88452819a4aabe2a29971ce86079ef7f0008
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732497"
---
# <a name="icordebugprocessenumnext-method"></a>ICorDebugProcessEnum::Next 方法

从当前位置开始，从枚举中获取指定数量的 ICorDebugProcess 实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next (  
    [in]  ULONG celt,  
    [out, size_is(celt), length_is(*pceltFetched)]  
        ICorDebugProcess *processes[],  
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 中 `ICorDebugProcess` 要检索的实例数。  
  
 `processes`  
 弄一个指针数组，其中每个指针指向一个 `ICorDebugProcess` 表示进程的对象。  
  
 `pceltFetched`  
 弄一个指针，指向 `ICorDebugProcess` 实际返回的实例数。 如果为1，则此值可以为 null `celt` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
