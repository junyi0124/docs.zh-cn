---
title: “Cor调试解码事件标志窗口”枚举
ms.date: 03/30/2017
api_name:
- CorDebugDecodeEventFlagsWindows
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: aa6cf962-30ae-4cfd-8895-826deeb46a54
topic_type:
- apiref
ms.openlocfilehash: 60eab923aac5dea927105e8ca9fa77eb5708f5ac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733355"
---
# <a name="cordebugdecodeeventflagswindows-enumeration"></a>“Cor调试解码事件标志窗口”枚举

提供关于 Windows 平台上的调试事件的其他信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorDebugDecodeEventFlagsWindows {  
    IS_FIRST_CHANCE = 1,  
} CorDebugDecodeEventFlagsWindows;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`IS_FIRST_CHANCE`|指出调试事件为最可能的异常。|  
  
## <a name="remarks"></a>注解  

 [ICorDebugProcess6：:D ecodeevent](icordebugprocess6-decodeevent-method.md)方法包含一个 `dwFlags` 参数，该参数提供有关调试事件的附加信息，并且其值依赖于目标体系结构。 `CorDebugDecodeEventFlagsWindows` 枚举可与 Windows 平台上的调试事件一起使用。  
  
> [!NOTE]
> 此枚举仅用于 .NET Native 调试方案。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
