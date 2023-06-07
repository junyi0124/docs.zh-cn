---
title: ICorDebugNativeFrame::CanSetIP 方法
ms.date: 03/30/2017
api_name:
- ICorDebugNativeFrame.CanSetIP
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugNativeFrame::CanSetIP
helpviewer_keywords:
- ICorDebugNativeFrame::CanSetIP method [.NET Framework debugging]
- CanSetIP method, ICorDebugNativeFrame interface [.NET Framework debugging]
ms.assetid: 13258ac6-f4e4-4f66-8fc3-f1244417a3c3
topic_type:
- apiref
ms.openlocfilehash: 194065e53d550c9bbd0486de54462309a4d9ffa1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95695733"
---
# <a name="icordebugnativeframecansetip-method"></a>ICorDebugNativeFrame::CanSetIP 方法

获取 HRESULT，它指示是否可以安全地将指令指针 (IP) 设置为本机代码中的指定偏移位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CanSetIP (  
    [in] ULONG32            nOffset  
);  
```  
  
## <a name="parameters"></a>参数  

 `nOffset`  
 中指令指针所需的设置。  
  
## <a name="remarks"></a>注解  

 在 `CanSetIP` 调用 [ICorDebugNativeFrame：： SetIP](icordebugnativeframe-setip-method.md) 方法之前，请使用方法。 如果 `CanSetIP` 返回除 S_OK 以外的任何 HRESULT，则仍可调用 `ICorDebugNativeFrame::SetIP` ，但是无法保证调试器将继续安全且正确地执行正在调试的代码。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅
