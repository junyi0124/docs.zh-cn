---
title: ICorDebugCode2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugCode2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugCode2
helpviewer_keywords:
- ICorDebugCode2 interface [.NET Framework debugging]
ms.assetid: 9321903b-7dea-40d8-ba32-99016c00cc46
topic_type:
- apiref
ms.openlocfilehash: 1e5b92d99d8ae52c88f1517f9c3d7db8e70598ac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720797"
---
# <a name="icordebugcode2-interface"></a>ICorDebugCode2 接口

提供扩展 "ICorDebugCode" 的功能的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetCodeChunks 方法](icordebugcode2-getcodechunks-method.md)|获取包含此代码对象的代码块。|  
|[GetCompilerFlags 方法](icordebugcode2-getcompilerflags-method.md)|获取一些标志，这些标志指定此代码对象是实时 (JIT) 使用本机映像)  ( 生成器编译或生成的。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugCode3 接口](icordebugcode3-interface.md)
- [调试接口](debugging-interfaces.md)
