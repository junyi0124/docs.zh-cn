---
title: IcorDebugVariableHome：： GetLiveRange 方法
ms.date: 03/30/2017
api_name:
- ICorDebugVariableHome.GetLiveRange
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugVariableHome::GetLiveRange
helpviewer_keywords:
- ICorDebugVariableHome::GetLiveRange method [.NET Framework debugging]
- GetLiveRange method, ICorDebugVariableFrame interface [.NET Framework debugging]
ms.assetid: 87277e1a-1595-4729-9e25-d1c3ac18ce5f
topic_type:
- apiref
ms.openlocfilehash: 4d66d09e02b907281f64400b0c605a7b5c44d476
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731041"
---
# <a name="icordebugvariablehomegetliverange-method"></a>IcorDebugVariableHome：： GetLiveRange 方法

获取此变量的生存期的本机范围。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetLiveRange(  
    [out] ULONG32* pStartOffset,  
    [out] ULONG32 *pEndOffset  
);  
```  
  
## <a name="parameters"></a>参数  

 `pStartOffset`  
 弄变量首次处于活动状态的逻辑偏移量。  
  
 `pEndOffset`  
 弄紧跟在变量上一次生存点之后的逻辑偏移量。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)
