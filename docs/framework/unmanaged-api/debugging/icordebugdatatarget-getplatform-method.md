---
title: ICorDebugDataTarget::GetPlatform 方法
ms.date: 03/30/2017
api_name:
- ICorDebugDataTarget.GetPlatform Method
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugDataTarget::GetPlatform
helpviewer_keywords:
- GetPlatform method [.NET Framework debugging]
- ICorDebugDataTarget::GetPlatform method [.NET Framework debugging]
ms.assetid: 9ee96c9d-7a3d-4129-a6cc-7675c7f2dda4
topic_type:
- apiref
ms.openlocfilehash: e8612b23cbfa506fddb2fad712848b285b9ac28e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679788"
---
# <a name="icordebugdatatargetgetplatform-method"></a>ICorDebugDataTarget::GetPlatform 方法

提供有关平台的信息，包括在其上运行目标进程的处理器体系结构和操作系统。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetPlatform([out] CorDebugPlatform * pTargetPlatform);  
```  
  
## <a name="parameters"></a>参数  

 `pTargetPlatform`  
 弄指向描述目标平台的 [CorDebugPlatformEnum](cordebugplatform-enumeration.md) 枚举的指针。  
  
## <a name="remarks"></a>注解  

 `CorDebugPlatformEnum`枚举返回值由[ICorDebug](icordebug-interface.md)接口用于确定目标进程的详细信息，例如，其指针大小、地址空间布局、寄存器集、指令格式、上下文布局和调用约定。  
  
 `pTargetPlatform`该值可以引用为目标模拟的平台，而不是指定使用中的实际硬件。 例如，在 windows 上 Windows (WOW) 环境的 windows 操作系统的 windows 操作系统64上运行的进程应使用 `CORDB_PLATFORM_WINDOWS_X86` [CorDebugPlatformEnum](cordebugplatform-enumeration.md) 枚举的值。  
  
 此方法必须成功。 如果该操作失败，目标平台将无法使用。 此方法可能会由于以下原因而失败：  
  
- 正在为目标模拟的平台不可用。  
  
- 目标平台上的实际硬件不可用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugDataTarget 接口](icordebugdatatarget-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
