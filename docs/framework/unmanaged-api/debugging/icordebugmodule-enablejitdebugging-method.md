---
title: ICorDebugModule::EnableJITDebugging 方法
ms.date: 03/30/2017
api_name:
- ICorDebugModule.EnableJITDebugging
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugModule::EnableJITDebugging
helpviewer_keywords:
- ICorDebugModule::EnableJITDebugging method [.NET Framework debugging]
- EnableJITDebugging method [.NET Framework debugging]
ms.assetid: 0a65e2a4-5bb6-496c-ae6f-40474426b5a6
topic_type:
- apiref
ms.openlocfilehash: 359db27878ea4adf794bcd6221d4b5387026e5c0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95710306"
---
# <a name="icordebugmoduleenablejitdebugging-method"></a>ICorDebugModule::EnableJITDebugging 方法

控制实时 (JIT) 编译器是否保留此模块内方法的调试信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnableJITDebugging(  
    [in] BOOL bTrackJITInfo,  
    [in] BOOL bAllowJitOpts  
);  
```  
  
## <a name="parameters"></a>参数  

 `bTrackJITInfo`  
 中将此值设置为 `true` ，以使 JIT 编译器能够在此模块中的每个方法的 Microsoft 中间语言 (MSIL) 版本和 JIT 编译版本之间保留映射信息。  
  
 `bAllowJitOpts`  
 中将此值设置为 `true` ，可让 jit 编译器生成代码，并使用特定于 JIT 的优化进行调试。  
  
## <a name="remarks"></a>注解  

 默认情况下，调试程序处于活动状态时加载的所有模块都已启用 JIT 调试。 以编程方式启用或禁用设置将覆盖全局设置。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
