---
title: ICorDebugClass2::SetJMCStatus 方法
ms.date: 03/30/2017
api_name:
- ICorDebugClass2.SetJMCStatus
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugClass2::SetJMCStatus
helpviewer_keywords:
- ICorDebugClass2::SetJMCStatus method [.NET Framework debugging]
- SetJMCStatus method, ICorDebugClass2 interface [.NET Framework debugging]
ms.assetid: 077e6c7f-f857-480c-bebb-76ee1de4e8fc
topic_type:
- apiref
ms.openlocfilehash: 1db2c9b5e65ae150f05242172f5ea16db433bbb5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717820"
---
# <a name="icordebugclass2setjmcstatus-method"></a>ICorDebugClass2::SetJMCStatus 方法

对于类的每个方法，设置一个值，该值指示该方法是否为用户定义的代码。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT SetJMCStatus (  
    [in] BOOL   bIsJustMyCode  
);  
```  
  
## <a name="parameters"></a>参数  

 `bIsJustMyCode`  
 中如果设置为，则 `true` 指示该方法是用户定义的代码; 否则设置为 `false` 。  
  
## <a name="remarks"></a>注解  

 ) 分档器 (JMC 的简单代码将跳过非用户定义的代码。 用户定义的代码必须是可调试代码的子集。  
  
 `SetJMCStatus` 如果无法为任何方法设置值，则返回 S_FALSE 的 HRESULT 值，即使它成功设置了所有其他方法的值也是如此。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
