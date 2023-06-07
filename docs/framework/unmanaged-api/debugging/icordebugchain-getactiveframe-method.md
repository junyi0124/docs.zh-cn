---
title: ICorDebugChain::GetActiveFrame 方法
ms.date: 03/30/2017
api_name:
- ICorDebugChain.GetActiveFrame
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugChain::GetActiveFrame
helpviewer_keywords:
- ICorDebugChain::GetActiveFrame method [.NET Framework debugging]
- GetActiveFrame method, ICorDebugChain interface [.NET Framework debugging]
ms.assetid: 36887017-670b-4f21-b406-8fab956f84a3
topic_type:
- apiref
ms.openlocfilehash: daecd216b4d7e9c23336b8956c13735549be901b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730131"
---
# <a name="icordebugchaingetactiveframe-method"></a>ICorDebugChain::GetActiveFrame 方法

获取当前链上的活动 (，即最近) 的帧。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetActiveFrame (  
    [out] ICorDebugFrame   **ppFrame  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppFrame`  
 弄指向 ICorDebugFrame 对象的地址的指针，该对象表示活动 (，即链上最近) 的帧。  
  
## <a name="remarks"></a>注解  

 如果没有可用的托管堆栈帧， `ppFrame` 则将设置为 null。  
  
 如果活动帧不可用，则调用将成功，并且 `ppFrame` 将为 null。 由于 CHAIN_ENTER_UNMANAGED，活动帧将不可用于发起的链，并且由于 CHAIN_CLASS_INIT 而导致的某些链启动了。 请参阅 CorDebugChainReason 枚举。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]
